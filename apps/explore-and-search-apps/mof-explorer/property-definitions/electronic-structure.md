---
description: How were electronic structure properties computed?
---

# Electronic Structure

Band gaps are computed using Pymatgen's [EIGENVAL parser](https://pymatgen.org/pymatgen.io.vasp.outputs.html#pymatgen.io.vasp.outputs.Eigenval.eigenvalue\_band\_properties), which uses the Kohn-Sham eigenvalues to compute the energy gap. In all cases, the displayed band gap is from a self-consistent calculation. We note that band gaps using the PBE functional are typically underpredicted compared to experiment. Although available for only a portion of the QMOF Database, band gaps calculated with the HSE06 functional are likely to be more accurate.

Density of states: TBD.
