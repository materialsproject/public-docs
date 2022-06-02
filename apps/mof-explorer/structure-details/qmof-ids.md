---
description: What is a QMOF ID?
---

# QMOF IDs

Each material in the QMOF Database is assigned a unique 7-digit QMOF ID that is associated with that material. All calculations associated with a given QMOF ID are for a given PBE-D3(BJ) optimized structure. Each QMOF ID represents a unique structure, as determined using Pymatgen's [StructureMatcher](https://pymatgen.org/pymatgen.analysis.structure\_matcher.html#pymatgen.analysis.structure\_matcher.StructureMatcher). As such, the primitive unit cells of any two structures are distinct after any relevant volume rescaling. Depending on your personal definition of a unique material, you may wish to further unique-ify the structures. For instance, MOFs with closed-pore and open-pore configurations would be considered unique, MOFs with different linker configurations would be considered unique, and so on.
