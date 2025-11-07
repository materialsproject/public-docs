# Adding Energy Corrections to Custom Entries

To use the outputs of custom DFT calculations in conjunction with the entries from the Materials Project ([provided the custom calculations are run with the same settings](../../../calculation-details/)) , a [series of corrections ](gga-gga+u-r2scan-mixing.md)have to applied to the outputs of the calculation:&#x20;

```python
from pymatgen.io.vasp import Vasprun
from pymatgen.entries.computed_entries import ComputedStructureEntry
from pymatgen.entries.compatibility import MaterialsProject2020Compatibility

vasprun = Vasprun("vasprun.xml")
processed_entry = ComputedStructureEntry(
    structure = vasprun.structures[-1],
    energy = vasprun.final_energy,
    parameters = {'run_type': 'GGA', 'potcar_symbols': vasprun.potcar_symbols}
)
processed_entry.energy_adjustments = MaterialsProject2020Compatibility().get_adjustments(cse)
```

For structures containing anions, [another correction](anion-and-gga-gga+u-mixing.md) has to be applied by supplying oxidation states:

```python
from pymatgen.entries.compatibility import MaterialsProject2020Compatibility
from pymatgen.analysis.bond_valence import BVAnalyzer

#if oxidation states are not known a-priori, use Bond Valences to assign them
try:
    bva = BVAnalyzer()
    entry.data["oxidation_states"] = {
        site.species.elements[0].name : bva.get_valences(entry.structure)[i]
        for i, site in enumerate(entry.structure)
    }
except Exception as exc:
    oxi_states = entry.composition.oxi_state_guesses()
    if len(oxi_states) > 0:
        entry.data["oxidation_states"] = oxi_states[0]

processed_entry = MaterialsProject2020Compatibility()
processed_entry.process_entry(entry)
```

Then the computed structure entry will contain an individual list of energy corrections, accessible through `processed_entry.energy_adjustments`, an overall correction value in `processed_entry.corrections`, and a final corrected energy in `processed_entry.energy` or `processed_entry.energy_per_atom` .
