# Adding Energy Corrections to Custom Entries

To use the outputs of custom DFT calculations (provided they are done with the same input set) in conjunction with the entries from the Materials Project, a series of corrections have to applied to the calculation:&#x20;

```python
from pymatgen.io.vasp import Vasprun
from pymatgen.entries.computed_entries import ComputedStructureEntry
from pymatgen.entries.compatibility import MaterialsProject2020Compatibility

vasprun = Vasprun("vasprun.xml")
cse = ComputedStructureEntry(
    structure = vasprun.structures[-1],
    energy = vasprun.final_energy,
    parameters = {'run_type': 'GGA', 'potcar_symbols': vasprun.potcar_symbols}
)
cse.energy_adjustments = MaterialsProject2020Compatibility().get_adjustments(cse)
```
