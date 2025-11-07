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
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    summary_doc = mpr.materials.summary.search(material_ids=["mp-149"])[0]
    task_id = str(summary_doc.dos.total["1"].task_id)
    task_doc = mpr.materials.tasks.search(task_ids=[task_id])[0]
print(task_doc.input.parameters.get("NELECT"))
```

> _**NOTE:**_ Be aware that the POTCARs we use in calculations has changed over time, the value of `NELECT` _is not always determined_ by the `MPRelaxSet`. If a DOS was generated with r2SCAN, then the right set to use is `MPScanRelaxSet`. The method above circumvents this by letting you directly retrieve the value of `NELECT`.

### Get task-id associated with DOS (mp-149)

The task-id can indicate what functional was used to generate the DOS.

```psl
with MPRester() as mpr:
    # get all electronic structure info:
    estruct_doc = mpr.materials.electronic_structure.search(material_ids=["mp-149"])[0]

    # Inspect task IDs associated with the electronic structure document
    print(f"DOS task ID = {estruct_doc.dos.total['1'].task_id}")
    print(f"Band structure task ID = {estruct_doc.task_id}")

    # Retrieve the task corresponding to the electronic DOS:
    dos_task = mpr.materials.tasks.search(task_ids=[estruct_doc.dos.total["1"].task_id])[0]

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
with MPRester() as mpr:
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
from mp_api.client import MPRester with MPRester() as mpr: robocrys_docs = mpr.materials.robocrys.search(keywords=["perovskite"]) robo_perov_mpids = [doc.material_id for doc inrobocrys_docs]
```
{% endcode %}

***

#### 2. Search by Provenance Tags and Remarks

Many materials have "perovskite" in their tags or remarks fields:

{% code overflow="wrap" %}
```python
with MPRester(use_document_model=False) as mpr: prov_docs = mpr.materials.provenance.search( fields=["material_id", "remarks", "tags"] ) possible_perov = [ doc.get("material_id") for doc in prov_docs ifany("perovskite" in tag.lower() for tag in (doc.get("tags", []) + doc.get("remarks", []))) ]
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

{% code overflow="wrap" %}
```python
with MPRester() as mpr: summaries = mpr.materials.summary.search( material_ids=likely_perovskite_mpids[:10] ) for summary in summaries: print(summary.formula_pretty, summary.material_id)
```
{% endcode %}



