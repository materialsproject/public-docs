---
description: >-
  Some nuances about structures in the QMOF Database (and all MOF databases, in
  fact)
---

# Structural Fidelity

As described in the original [QMOF Database paper](https://doi.org/10.1016/j.matt.2021.02.015), the structural fidelity of MOF crystal structures is an incredibly challenging but important factor to consider when constructing DFT-based property databases. Many experimental MOF crystal structures have missing atoms (e.g. missing H atoms), under or overbonded atoms, unresolved disorder, charge-imbalances (e.g. missing or too many ions), and related issues. Similarly, some hypothetical MOF databases have building blocks with under or overbonded carbon atoms due to faulty functionalization routines. While significant effort was put into maximizing the structural fidelity of materials on the QMOF Database, we acknowledge that there are inevitably structures in the database that are not pristine.

If you find a material with poor structural fidelity, we ask you to [open an issue](https://github.com/arosen93/QMOF/issues) listing the QMOF IDs of any problematic structures along with an explanation of the structural error. While we are not in a position to correct structures at this time, they will be removed from the QMOF Database when identified by the community, and a new version of the database will be minted.
