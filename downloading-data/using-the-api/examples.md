---
description: API query examples with the MPRester client.
---

# Examples

## Summary Queries

### Structure data for silicon (mp-149)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:

    docs = mpr.summary.search(material_ids=["mp-149"], fields=["structure"])
    structure = docs[0].structure
    
    # -- Shortcut for a single Materials Project ID:
    structure = mpr.get_structure_by_material_id("mp-149")
```

### Find all Materials Project IDs for entries with dielectric data

```python
from mp_api.client import MPRester
from emmet.core.summary import HasProps

with MPRester("your_api_key_here") as mpr: 

    docs = mpr.summary.search(has_props = [HasProps.dielectric], fields=["material_id"])
    mpids = [doc.material_id for doc in docs]
```

### Calculation (task) IDs and types for silicon (mp-149)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr: 

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
    docs = mpr.summary.search(chemsys="Si-O", 
                              fields=["material_id", "band_gap"])
    mpid_bgap_dict = {doc.material_id: doc.band_gap for doc in docs}
```

### Chemical formulas for all materials containing _at least_ Si and O

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.summary.search(elements=["Si", "O"], 
                              fields=["material_id", "band_gap"])
    mpid_formula_dict = {doc.material_id: doc.pretty_formula for doc in docs}
```

### Stable materials (on the hull) with large band gaps (>3eV)

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    docs = mpr.summary.search(band_gap=(3,None),
                              is_stable=True, 
                              fields=["material_id"])
    stable_mpids = [doc.material_id for doc in docs]
    
    ## -- Alternative directly using energy above hull:
    docs = mpr.summary.search(band_gap=(3,None),
                              energy_above_hull=(0,0),
                              fields=["material_id"])
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

### XAS for TiO2 element O K edge:&#x20;

```python
from mp_api.client import MPRester
from emmet.core.xas import Edge, XASDoc, Type

with MPRester("your_api_key_here") as mpr:
    xas = mpr.xas.search_xas_docs(formula = "TiO2", 
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
    pd = mpr.thermo.get_phase_diagram_from_chemsys(chemsys="Li-Fe-O", 
                                                   thermo_type=ThermoType.GGA_GGA_U_R2SCAN")
    
    # -- GGA/GGA+U mixed phase diagram
    pd = mpr.thermo.get_phase_diagram_from_chemsys(chemsys="Li-Fe-O", 
                                                   thermo_type=ThermoType.GGA_GGA_U")
                                                   
    # -- R2SCAN only phase diagram
    pd = mpr.thermo.get_phase_diagram_from_chemsys(chemsys="Li-Fe-O", 
                                                   thermo_type=ThermoType.R2SCAN")
   
    
```
