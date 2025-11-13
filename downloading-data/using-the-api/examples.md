---
description: API query examples with the MPRester client.
---

# Examples

## Summary Queries

### Structure data for silicon (mp-149)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.materials.summary.search(material_ids=["mp-149"], fields=["structure"])
    structure = docs[0].structure
    # -- Shortcut for a single Materials Project ID:
    structure = mpr.get_structure_by_material_id("mp-149")
```

### Querying ICSD ID

For structures tagged with at least one ICSD entry, the simplest way to query structure with ICSD ID is this:&#x20;

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    mp_docs = mpr.materials.summary.search(fields=["material_id", "database_IDs"])

icsd_to_mpid = {}
for mp_doc in mp_docs:
    mpid = str(mp_doc.material_id)
    for icsd_id in mp_doc.database_IDs.get("icsd",[]):
        if icsd_id not in icsd_to_mpid:
            icsd_to_mpid[icsd_id] = []
        icsd_to_mpid[icsd_id].append(mpid)
```

Then the keys of `icsd_to_mpid` will be all ICSD IDs currently matched to at least one entry in MP, and its values  will be the MP IDs which structurally match to that ICSD ID.

> _**NOTE:**_ Not every ICSD entry is included in Materials Project - some of them we’re working on adding, others we do not plan to add (e.g., if they are disordered with a complex disordering ratio). Furthermore, many ICSD entries can structure match to the same MP ID. We use the pymatgen StructureMatcher to determine structural similarity

### Find all Materials Project IDs for entries with dielectric data

```python
from mp_api.client import MPRester
from emmet.core.summary import HasProps

with MPRester("your_api_key_here") as mpr:
    docs = mpr.materials.summary.search(
        has_props = [HasProps.dielectric], fields=["material_id"]
    )
    mpids = [doc.material_id for doc in docs]
```

### Calculation (task) IDs and types for silicon (mp-149)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr: 
    # use core rester
    docs = mpr.materials.search(material_ids=["mp-149"], fields=["calc_types"])
    task_ids = docs[0].calc_types.keys()
    task_types = docs[0].calc_types.values() 
    # -- Shortcut for a single Materials Project ID:
    task_ids = mpr.get_task_ids_associated_with_material_id("mp-149")
```

### Band gaps for all materials containing _only_ Si and O

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.materials.summary.search(
        chemsys="Si-O", fields=["material_id", "band_gap"]
    )
    mpid_bgap_dict = {doc.material_id: doc.band_gap for doc in docs}
```

### Chemical formulas for all materials containing _at least_ Si and O

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.materials.summary.search(
        elements=["Si", "O"], fields=["material_id", "band_gap", "formula_pretty"]
    )
    mpid_formula_dict = {
        doc.material_id: doc.formula_pretty for doc in docs
    }
```

### Material IDs for all ternary oxides with the form ABC3

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.materials.summary.search(
        chemsys="O-*-*", formula="ABC3",
        fields=["material_id"]
    )
    mpids = [doc.material_id for doc in docs]
```

### Stable materials (on the GGA/GGA+U hull) with large band gaps (>3eV)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.materials.summary.search(
        band_gap=(3, None), is_stable=True, fields=["material_id"]
    )
    stable_mpids = [doc.material_id for doc in docs]
    
    ## -- Alternative directly using energy above hull:
    docs = mpr.materials.summary.search(
        band_gap=(3, None), energy_above_hull=(0, 0), fields=["material_id"]
    )
    stable_mpids = [doc.material_id for doc in docs]
```

## Electronic Structure

### Band structures for silicon (mp-149)

```python
from mp_api.client import MPRester
from emmet.core.electronic_structure import BSPathType

with MPRester("your_api_key_here") as mpr:
    # -- line-mode, Setyawan-Curtarolo (default):
    bs_sc = mpr.get_bandstructure_by_material_id("mp-149")
    
    # -- line-mode, Hinuma et al.:
    bs_hin = mpr.get_bandstructure_by_material_id("mp-149", path_type=BSPathType.hinuma)

    # -- line-mode, Latimer-Munro:
    bs_hin = mpr.get_bandstructure_by_material_id("mp-149", path_type=BSPathType.latimer_munro)
    
    # -- uniform:
    bs_uniform = mpr.get_bandstructure_by_material_id("mp-149", line_mode=False)                            
```

### Density of states for silicon (mp-149)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    dos = mpr.get_dos_by_material_id("mp-149")
    
# Use pymatgen's features to normalize the DOS
normalized_dos = dos.get_normalized()

# or use the associated structure in the DOS
norm_vol = dos.structure.volume
```

### VASP Input Parameters (e.g. NELECT)

To get `NELECT` (or any other INCAR parameters) is by getting the `task_id` for the entry and then querying the tasks endpoint directly.

Suppose you want the value of `NELECT` for `mp-149`:

```python
from pymatgen.electronic_structure.core import Spin
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    summary_doc = mpr.materials.summary.search(material_ids=["mp-149"])[0]
    task_id = str(summary_doc.dos.total[Spin.up].task_id)
    task_doc = mpr.materials.tasks.search(task_ids=[task_id])[0]
print(task_doc.input.parameters.get("NELECT"))
```

> _**NOTE:**_ Be aware that the POTCARs we use in calculations has changed over time, the value of `NELECT` _is not always determined_ by the `MPRelaxSet`. If a DOS was generated with r2SCAN, then the right set to use is `MPScanRelaxSet`. The method above circumvents this by letting you directly retrieve the value of `NELECT`.

### Get task-id associated with DOS (mp-149)

The task-id can indicate what functional was used to generate the DOS.

```psl
from pymatgen.electronic_structure.core import Spin

with MPRester() as mpr:
    # get all electronic structure info:
    estruct_doc = mpr.materials.electronic_structure.search(material_ids=["mp-149"])[0]

    # Inspect task IDs associated with the electronic structure document
    print(f"DOS task ID = {estruct_doc.dos.total[Spin.up].task_id}")
    print(f"Band structure task ID = {estruct_doc.task_id}")

    # Retrieve the task corresponding to the electronic DOS:
    dos_task = mpr.materials.tasks.search(task_ids=[estruct_doc.dos.total[Spin.up].task_id])[0]

print(dos_task.task_id,dos_task.calc_type)

```

## Phonons

### Band structure for silicon (mp-149)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    ph_bs = mpr.get_phonon_bandstructure_by_material_id("mp-149")
```

### Density of states for silicon (mp-149)

```python
from mp_api.client import MPRester
from emmet.core.electronic_structure import BSPathType

with MPRester("your_api_key_here") as mpr:
    ph_dos = mpr.get_phonon_dos_by_material_id("mp-149")
```

## XAS

### XAS for TiO2 element O K edge:

```python
from mp_api.client import MPRester
from emmet.core.xas import Edge, XASDoc, Type

with MPRester("your_api_key_here") as mpr:
    xas = mpr.materials.xas.search(formula = "TiO2", 
                                  absorbing_element = 'Ti', 
                                  edge = Edge.K)

```

## Charge Density

### Charge density for silicon (mp-149)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    chgcar = mpr.get_charge_density_from_material_id("mp-149")
```

## Phase Diagram

### Phase diagram for the Li-Fe-O chemical system

```python
from mp_api.client import MPRester
from emmet.core.thermo import ThermoType

with MPRester("your_api_key_here") as mpr:
    
    # -- GGA/GGA+U/R2SCAN mixed phase diagram
    pd = mpr.materials.thermo.get_phase_diagram_from_chemsys(chemsys="Li-Fe-O", 
                                                   thermo_type=ThermoType.GGA_GGA_U_R2SCAN)
    
    # -- GGA/GGA+U mixed phase diagram
    pd = mpr.materials.thermo.get_phase_diagram_from_chemsys(chemsys="Li-Fe-O", 
                                                   thermo_type=ThermoType.GGA_GGA_U)
                                                   
    # -- R2SCAN only phase diagram
    pd = mpr.materials.thermo.get_phase_diagram_from_chemsys(chemsys="Li-Fe-O", 
                                                   thermo_type=ThermoType.R2SCAN)
   
    
```

## Querying amorphous materials

There are some caveats with representing amorphous structures as small ordered unit cells, and the resulting structures may be phase separated or not representative of a true amorphous phase. Nevertheless, the Materials Project has "amorphous" entries for some compositions, which can be queried and filtered.\
\
The following is an example of how to obtain the Materials Project IDs for all SiO2 "amorphous" solids:

```python
from mp_api.client import MPRester
from pymatgen.core import Composition

target_composition = Composition({"Si": 1, "O": 2})
with MPRester("your_api_key_here") as mpr:
    si_o_mpids = [
        doc.material_id
        for doc in mpr.materials.search(chemsys="Si-O", fields = ["material_id","composition_reduced"])
        if doc.composition_reduced == target_composition
    ]
    si_o_prov = mpr.materials.provenance.search(material_ids=si_o_mpids)
amorphous_si_o2_mpids = [doc.material_id.string for doc in si_o_prov if any("amorphous" in tag.lower() for tag in doc.tags)]
```

### Searching by Crystal Prototype: Example — Perovskite

Searching for materials by formula (e.g., `ABC₃`) can miss important structure types like perovskites with non-standard formulas (e.g., Cs₃Sb₂I₉). Instead, you can search by **crystal prototype** or structure type using the Materials Project API.

#### 1. Search by Robocrystallographer Description

Robocrystallographer generates structure descriptions, including the term "perovskite" for relevant materials:

{% code overflow="wrap" %}
```python
from mp_api.client import MPRester
with MPRester("your_api_key_here") as mpr:
    robocrys_docs = mpr.materials.robocrys.search(keywords=["perovskite"])
robo_perov_mpids = [doc.material_id for doc in robocrys_docs])
```
{% endcode %}

***

#### 2. Search by Provenance Tags and Remarks

Many materials have "perovskite" in their tags or remarks fields:

{% code overflow="wrap" %}
```python
with MPRester("your_api_key_here",use_document_model=False) as mpr:
    prov_docs = mpr.materials.provenance.search( fields=["material_id", "remarks", "tags"] ) possible_perov = [
    doc.get("material_id") for doc in prov_docs
    if any("perovskite" in tag.lower() for tag in (doc.get("tags", []) + doc.get("remarks", []))) 
]
```
{% endcode %}

***

#### 3. Combine Results

Merge both lists for a comprehensive set of perovskite materials:

{% code overflow="wrap" %}
```python
likely_perovskite_mpids = list(set(robo_perov_mpids).union(possible_perov))
```
{% endcode %}

***

#### 4. (Optional) Fetch Structures

<pre class="language-python" data-overflow="wrap"><code class="lang-python">with MPRester("your_api_key_here") as mpr:
    summaries = mpr.materials.summary.search(material_ids=likely_perovskite_mpids)
<strong>for summary in summaries:
</strong><strong>    print(summary.formula_pretty, summary.material_id)
</strong></code></pre>

## Querying specialized calculations like DFPT

MP contains specialized calculations to compute various materials properties. Sometimes it's of interest to find those calculations. A full list of valid such "task types" are given in our builder software, [emmet](https://github.com/materialsproject/emmet/blob/6b2b8492edfcab6e81f29b5eda1eb938d0724160/emmet-core/emmet/core/vasp/calc_types/enums.py#L87).

**DFPT outputs and data only exist for parts of our database.** The following code snippet will take only relevant task (single DFT calculation) data from our database and check to see if it’s a DFPT calculation:&#x20;

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here",use_document_model=False,monty_decode=False) as mpr:
    tasks = mpr.materials.tasks.search(fields=["task_id","task_type"])
    
dfpt_tasks = {
    task["task_id"] for task in tasks if task["task_type"] in {"DFPT", "DFPT Dielectric"}
}
```

`dfpt_tasks` will contain a set of task IDs which you can then query like `mpr.materials.tasks.search(task_ids=dfpt_tasks)`

### Identifying Materials with Specific Structural Dimensionalities

Though the Materials Project does not store structure dimension as a formal property, it is possible to  categorize entries by their structure dimension (1D, 2D, and 3D) by parsing [robocrystallographer](https://hackingmaterials.lbl.gov/robocrystallographer/index.html) analyses, which are generated for most materials in the Materials Project.

#### Finding Materials by Dimension in a Chemical System

Here's an example for the Mo-S system. Note how separate queries are used to retrieve material documents and robocrystallographer entries. These two entities must be associated with each other manually.

```python
from mp_api.client import MPRester

chemsys = "Mo-S"
with MPRester("your_api_key") as mpr:
    mat_docs = mpr.materials.search(chemsys=chemsys, fields=["material_id","structure"])
    robo_docs = mpr.materials.robocrys.search_docs(material_ids=[doc.material_id for doc in mat_docs])

mpid_to_structure = {doc.material_id.string: doc.structure for doc in mat_docs}
structures_by_dim = {"1D": {}, "2D": {}, "3D": {}}
int_to_word = {1: "one", 2: "two"}

for doc in robo_docs:
    for i in range(1, 3):
        if f"{int_to_word[i]}-dimensional" in doc.description.lower():
            dim = i
            break
        else:
            dim = 3
    structures_by_dim[f"{dim}D"][doc.material_id.string] = {
        "structure": mpid_to_structure[doc.material_id.string],
        "description": doc.description,
    }
```

#### Searching for All Dimensional Forms

To find chemical systems with materials existing in all three forms (1D, 2D, and 3D):

```python
from mp_api.client import MPRester
from collections import defaultdict

with MPRester("your_api_key") as mpr:
    mat_docs = mpr.materials.search(fields=['composition', 'material_id'])
    robo_docs = mpr.materials.robocrys.search_docs()

description_by_mpid = {doc.material_id: doc.description for doc in robo_docs}
formula_to_mpids = defaultdict(list)

for doc in mat_docs:
    formula = doc.composition.reduced_formula
    mpid = doc.material_id
    formula_to_mpids[formula].append(mpid)

mpid_to_dim = {}
int_to_word = {1: "one", 2: "two"}

for mpid, desc in description_by_mpid.items():
    for i in range(1, 3):
        if f"{int_to_word[i]}-dimensional" in desc.lower():
            mpid_to_dim[mpid] = f"{i}D"
            break
        else:
            mpid_to_dim[mpid] = "Other"

results = {}
for formula, mpids in formula_to_mpids.items():
    dim_to_mpids = {"1D": [], "2D": [], "Other": []}
    for mpid in mpids:
        if mpid in mpid_to_dim:
            dim = mpid_to_dim[mpid]
            dim_to_mpids[dim].append(mpid)
    if all(dim_to_mpids[d] for d in ["1D", "2D", "Other"]):
        results[formula] = dim_to_mpids

for i, (formula, dims) in enumerate(results.items(), 1):
    print(f"{i}- Chemical system: {formula}")
    for dim in ["1D", "2D", "Other"]:
        mpids = dims[dim]
        print(f"   - {len(mpids)} {dim} structures:")
        for mpid in mpids:
            print(f"       - {mpid}")
    print()
```
