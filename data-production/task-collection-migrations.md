# Task Collection Migrations

{% hint style="info" %}
DISCLAIMER: Make backups, or generate copies, of any applicable collections/databases before applying destructive operations
{% endhint %}

{% hint style="info" %}
Given the wide range of possible outputs from DFT workflows, this set of migrations should not be considered exhaustive.

These migrations are the steps that were taken by the Materials Project staff to migrate MP's core task document collection.
{% endhint %}

The schema of the task documents generated during calculation parsing has changed over time as the underlying workflows and workflow management software have evolved. Below are the migrations that were required for migrating the Materials Project's 10+ year old core task collection from the [TaskDocument](https://github.com/materialsproject/emmet/blob/6b2b8492edfcab6e81f29b5eda1eb938d0724160/emmet-core/emmet/core/vasp/task_valid.py#L100) produced by `atomate`'s [VaspDrone](https://github.com/hackingmaterials/atomate/blob/2e541f297165254ac054b50e8132adaa282a01e0/atomate/vasp/drones.py#L50), to `emmet-core'`s [TaskDoc](https://github.com/materialsproject/emmet/blob/6b2b8492edfcab6e81f29b5eda1eb938d0724160/emmet-core/emmet/core/tasks.py#L408) (used by [atomate2](https://github.com/materialsproject/atomate2)), and finally to the minified `emmet-core` [CoreTaskDoc](https://github.com/materialsproject/emmet/blob/6b2b8492edfcab6e81f29b5eda1eb938d0724160/emmet-core/emmet/core/tasks.py#L269) (the schema of documents returned by the `tasks` endpoint of the `mp-api` client).

#### Server-Side migrations

A few simple field re-names and type coercions were necessary during the transition from `TaskDocument` to `TaskDoc` that require no client-side data manipulations. These operations can be safely applied prior to migrating to `TaskDoc` to make the process smoother. These operations can also be applied to an existing `TaskDoc` collection.

* Migration of Potcar Symbols for `orig_inputs`
  * During the process of updating the `CalculationInput` class in `emmet-core`, a number of inconsistencies were found for the `potcar` field of `orig_inputs` (ref: [emmet PR comment](https://github.com/materialsproject/emmet/pull/1226#issuecomment-2836069648)). These updates will address those inconsistencies:

<pre class="language-python"><code class="lang-python">from pymongo import UpdateMany

<strong>ops = [
</strong>    # Migrate any erroneously parsed emmet.core.vasp.calculation.PotcarSpec's
    # to the correct location
    UpdateMany(
        {
            "$and": [
                {"orig_inputs.potcar": {"$type": "array"}},
                {"orig_inputs.potcar.0": {"$type": "object"}},
            ]
        },
        {
            "$rename": {"orig_inputs.potcar": "orig_inputs.potcar_spec"},
        },
    ),
    # Migrate emmet.core.Potcar (deprecated) struct -> list of potcar symbols
    UpdateMany(
        {"orig_inputs.potcar": {"$type": {"object"}}},
        {"$set": {"orig_inputs.potcar": {"$orig_inputs.potcar.symbols"}}},
    ),
]
</code></pre>

* Migration of `has_vasp_completed`
  * The transition from `atomate` to `atomate2` resulted in the `has_vasp_completed` field changing from a boolean to an enumeration. These operations will correct any mis-typed fields:

```python
from pymongo import UpdateMany

ops = [
    UpdateMany(
        {},
        {"$set": {"calcs_reversed.$[elem].has_vasp_completed": "successful"}},
        array_filters=[{"elem.has_vasp_completed": True}],
    ),
    UpdateMany(
        {},
        {"$set": {"calcs_reversed.$[elem].has_vasp_completed": "failed"}},
        array_filters=[{"elem.has_vasp_completed": False}],
    ),
]
```

* Removal of dropped `input` and `orig_inputs` fields
  * A number of fields were dropped in favor turning these values into `@property` s on the underlying `emmet.core.vasp.Calculation` class (`emmet-core` [#1226](https://github.com/materialsproject/emmet/pull/1226)). These fields can be safely removed:

```python
from pymongo import UpdateMany

ops = [
    UpdateMany(
        {},
        {
            "$unset": {
                "orig_inputs.poscar": "",
                "input.pseudo_potentials": "",
                "input.xc_override": "",
                "input.is_lasph": "",
                "input.magnetic_moments": "",
            }
        },
    )
]
```

The preceding operations can be executed individually, or concatenated and executed all at once. Consult the MongoDB [bulk write documentation](https://www.mongodb.com/docs/languages/python/pymongo-driver/current/crud/bulk-write/) for examples of executing bulk writes.

#### Client-Side Migrations

The following migrations involve more complicated manipulations and can have long run times depending on the size of the source tasks collection.

* Migration of `TaskDocument` to `TaskDoc`
  * The following `create_new_taskdoc` function can be used to transform a `TaskDocument` into a document with the `TaskDoc` schema. Coordination of database operations in this situation are highly dependent on the execution environment and are thus left to the user.

```python
from collections import abc

from emmet.core.tasks import TaskDoc
from emmet.core.utils import jsanitize
from emmet.core.types.enums import VaspObject


def recurse_list(seq):
    for item in seq:
        if isinstance(item, abc.Mapping):
            recurse_dict(item)
        elif isinstance(item, list):
            recurse_list(item)


def recurse_dict(nested):
    for key, value in nested.items():
        if isinstance(value, abc.Mapping):
            if not value:
                nested[key] = None
            else:
                recurse_dict(value)
        elif isinstance(value, str):
            if value == "None" or value == "":
                nested[key] = None
        elif isinstance(value, list):
            if len(value) == 0:
                nested[key] = None
            else:
                recurse_list(value)


def scrub_dict(entry):
    recurse_dict(entry)


def create_new_taskdoc(task_document):
    # A commonly found field mangling for TaskDocuments:
    if task_document["calcs_reversed"][0]["output"]["efermi"] == "None":
        task_document["calcs_reversed"][0]["output"]["efermi"] = None

    # Some very old TaskDocuments use "crystal" instead of "structure"
    try:
        struct_check = task_document["calcs_reversed"][0]["output"]["structure"]
    except KeyError:
        task_document["calcs_reversed"][0]["output"]["structure"] = task_document[
            "calcs_reversed"
        ][0]["output"]["crystal"]

    # Best to walk the document and coerce null-likes, empty strings,
    # and empty containers to null to have a consistent starting point
    scrub_dict(task_document)
    output_calc = task_document["calcs_reversed"][0]

    # First pass at splatting in fields, allows pydantic validators
    # to catch some common migrations
    temp_td = TaskDoc(**task_document)

    # OPTIONAL (commment out if not applicable):
    # In the past MP embedded file store refs (GridFS, object stores, etc.) for
    # large objects that were stored separately to keep document sizes managable
    # for db performance. This is a good place to migrate any bespoke fields to
    # a consistant field/format.
    # See emmet.core.types.enums.VaspObject for enumeration of commmon VASP Objects
    temp_td.vasp_objects = {}
    if "dos_fs_id" in output_calc:
        temp_td.vasp_objects[VaspObject.DOS] = {
            "dos_fs_id": output_calc["dos_fs_id"],
            "dos_compression": output_calc["dos_compression"],
        }

    if "bandstructure_fs_id" in output_calc:
        temp_td.vasp_objects[VaspObject.BANDSTRUCTURE] = {
            "bandstructure_fs_id": output_calc["bandstructure_fs_id"],
            "bandstructure_compression": output_calc["bandstructure_compression"],
        }

    dict_input = temp_td.model_dump()
    dict_input.update({"meta_structure": dict_input["structure"]})

    # run full TaskDoc constructor method
    task_doc_2 = TaskDoc.from_structure(**dict_input)

    final_task_dict = jsanitize(
        task_doc_2.model_dump(
            # TaskDoc pydantic model is extremely flexible and will 
            # accept extra fields, filter those out so they don't pollute
            # target collection
            include=set(TaskDoc.model_fields),
        )
    )

    return final_task_dict
```

#### Migration of `TaskDoc` to `CoreTaskDoc`

This is a multi-step process that should be done in order

1. Flattening `calcs_reversed`
   * Prior to `atomate2` , workflows would output multiple calculations into a single directory, leading to the need for a field like `calcs_reversed` that would allow for parsing multiple calculations in a single directory into a single task document. `atomate2` has done away with this and now even for complex, multi-step workflows each individual calculation has its own output directory. Extracting the individual calculations from `calcs_reversed` is straightforward:

```python
from pymongo.collection import Collection

source_collection: Collection
target_collection: Collection

# only worry about documents with len(calcs_reversed) > 1
query = {"$expr": {"$gt": [{"$size": "$calcs_reversed"}, 1]}}
projection = {"_id": 0}

pipeline = [{"$match": query}, {"$project": projection}]

batch_insert = []

for doc in source_collection.aggregate(pipeline):
    cr = doc.pop("calcs_reversed")

    # calcs_reversed is now a dict, not list[dict]
    # prepare for transition to CoreTaskDoc
    doc["calcs_reversed"] = cr[0]
    batch_insert.append(doc)

    for calc in cr[1:]:
        ...
        # dump additional calcs to disk
        # or directly utilize parsing logic defined
        # in next step and add to batch_insert


# can also do a find and modify on the source collection
# to modify the documents in place, but depending on the
# size of the updates this could be cpu/time intensive
target_collection.insert_many(batch_insert)
```

If the `source_collection` -> `target_collection` pattern is followed, also be sure to copy all documents from `source_collection` with `len(calcs_reversed) == 1` and set the `calcs_reversed` field equal to `calcs_reversed[0]` .

2. "Parsing" extracted entries from `calcs_reversed`
   * The `calcs_reversed` entries from step 1 will need to be transformed into their own `TaskDoc` documents:

```python
from emmet.core.tasks import TaskDoc
from emmet.core.utils import jsanitize

# load docs from disk from step 1, or use loop body in inner loop in step 1
docs = [...]
batch_insert = []

for doc in docs:
    # try to get a structure, if there is no structure in any 
    # of these places best to just discard the entry
    _struct = doc["output"]["structure"]
    if not _struct:
        _struct = doc["output"]["ionic_steps"][-1]["structure"] 
    if not _struct:
        _struct = doc["input"]["structure"]
    if not _struct:
        # best to set up some sort of logging for troubleshooting
        # logger.warning(f"No structure found for calc in {doc}")
        continue

    meta_structure=Structure.from_dict(_struct)

    task_doc = TaskDoc.from_structure(
        task_id=None, # New docs will have no task_id, can assign later
        # batch_id=..., optionally, assign a batch_id for easy identification
        meta_structure=meta_structure,
        calcs_reversed=[doc],
        input=doc["input"],
        output=doc["output"]
    )

    td = jsanitize(
        task_doc.model_dump(
            include=set(TaskDoc.model_fields),
        )
    )

    # calcs_reversed is now a dict, not list[dict]
    # prepare for transition to CoreTaskDoc
    td["calcs_reversed"] = td["calcs_reversed"][0]
    batch_insert.append(td)

target_collection.insert_many(batch_insert)
```

3. Flattened "`TaskDoc`" to `CoreTaskDoc`
   * Another core difference in `CoreTaskDoc` is the removal of the calculation's "trajectory" (energies/forces tracked across the ionic steps in the calculation: [ionic\_steps](https://github.com/materialsproject/emmet/blob/6b2b8492edfcab6e81f29b5eda1eb938d0724160/emmet-core/emmet/core/vasp/calculation.py#L620)) from the database entries to be stored externally. The size of the `ionic_steps` field can vary drastically across calculations and was found to be a major contributor to the on-disk storage size of MP's core task collection. See a full discussion here: emmet PR [#1232](https://github.com/materialsproject/emmet/pull/1232).
   * This snippet uses pyarrow to store the trajectories as parquet files as part of a pyarrow Dataset. Alternative storage formats can be substituted in as well

```python
import pyarrow as pa
import pyarrow.parquet as pq
from emmet.core.arrow import arrowize
from emmet.core.tasks import CoreTaskDoc
from emmet.core.trajectory import Trajectory
from emmet.core.vasp.calc_types import RunType, TaskType
from emmet.core.vasp.calculation import Calculation


def patch_output_structure(doc: CoreTaskDoc):
    """
    Patch magmoms into final structure for documents parsed with older methods
    """
    mag_density = (
        doc.output.outcar["total_magnetization"] / doc.output.structure.volume
        if doc.output.outcar["total_magnetization"]
        else None
    )

    if doc.output.outcar["magnetization"] is not None:
        magmoms = [m["tot"] for m in doc.output.outcar["magnetization"]]
        doc.output.structure.add_site_property("magmom", magmoms)

    doc.output.mag_density = mag_density


core_task_doc_collection: Collection

traj_dir = "<SOME TARGET DIR>"
# priority columns for external sorting later on
traj_priority = sorted(["run_type", "task_type", "identifier", "has_full_output"])

# get these from target_collection from steps 1 + 2
task_docs_with_flattened_calcs_reversed = [...]

docs = []
trajs = []
for doc in task_docs_with_flattened_calcs_reversed:
    task_id = doc["task_id"]
    cr = Calculation(**doc.pop("calcs_reversed"))
    tt = TaskType(doc["task_type"])
    rt = RunType(doc["run_type"])

    props = {
        "structure": [],
        "cart_coords": [],
        "electronic_steps": [],
        "energy": [],
        "e_wo_entrp": [],
        "e_fr_energy": [],
        "forces": [],
        "stress": [],
    }

    for ionic_step in cr.output.ionic_steps:
        props["structure"].append(ionic_step.structure)
        props["energy"].append(ionic_step.e_0_energy)
        for k in (
            "e_fr_energy",
            "e_wo_entrp",
            "forces",
            "stress",
            "electronic_steps",
        ):
            props[k].append(getattr(ionic_step, k))

    try:
        traj = Trajectory._from_dict(
            props, task_type=tt, run_type=rt, identifier=task_id
        )
    except Exception as e:
        ...
        # set up some logging ahead of time for troubleshooting
        # logger.warning(f"Failed to build trajectory for {doc['task_id']}")
        # logger.warning(f"Exception: {e}")
    else:
        _dict = traj.model_dump(mode="json")
        conv_data = traj.convergence_data
        for k, v in conv_data.items():
            _dict[f"{k}_convergence"] = v

        _dict["has_full_output"] = traj.has_full_output
        trajs.append(_dict)

    doc["output"] = cr.output
    doc["input"] = cr.input
    doc["task_id"] = task_id
    td = CoreTaskDoc(**doc)
    
    if td.output.outcar is not None:
        # Documents that were migrated to TaskDoc
        # from TaskDocument will likely be missing
        # some site properties on the final output 
        # structure that can be patched
        patch_output_structure(td)
        
    docs.append(td.model_dump())

    traj_schema = pa.schema(
        arrowize(Trajectory).fields
        + [
            pa.field("has_full_output", pa.bool_()),
            pa.field("energy_convergence", pa.list_(pa.float64())),
            pa.field("e_fr_energy_convergence", pa.list_(pa.float64())),
            pa.field("e_wo_entrp_convergence", pa.list_(pa.float64())),
            pa.field("forces_convergence", pa.list_(pa.float64())),
        ]
    )

# write CoreTaskDocs to mongo
core_task_doc_collection.insert_many(docs))

# write Trajectory pyarrow table to disk as pyarrow Dataset
traj_table = pa.Table.from_pylist(trajs)
traj_cols = traj_priority + [
    col for col in sorted(traj_table.column_names) if col not in traj_priority
]
traj_table = traj_table.select(traj_cols)
pq.write_to_dataset(traj_table, traj_dir)
```

{% hint style="info" %}
A dedicated method is available in `emmet-core` for constructing a list of `Trajectory` objects from a list of `Calculation` s: [get\_trajectories\_from\_calculations](https://github.com/materialsproject/emmet/blob/6b2b8492edfcab6e81f29b5eda1eb938d0724160/emmet-core/emmet/core/vasp/calculation.py#L1394)\
\
This may be a viable alternative to the loop above depending on the input data.
{% endhint %}

Depending on the size of the source tasks collection, this process can be time and cpu intensive and may need additional batch processing logic.
