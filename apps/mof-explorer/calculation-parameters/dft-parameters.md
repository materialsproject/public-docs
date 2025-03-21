---
description: >-
  Description of the density functional theory (DFT) parameters used in MOF
  calculation results displayed on the Materials Project (MP) website.
---

# DFT Parameters

We use density functional theory (DFT) as implemented in the Vienna Ab Initio Simulation Package (VASP) 5.4.4. All calculations are carried out at 0 K and 0 atm. The plane-wave kinetic energy cutoff was set to 520 eV, which is 1.3 times the highest cutoff recommended among the PAW PBE pseudopotentials we use. Unless stated otherwise, we used a _k_-point mesh of 1000/(number of atoms per cell), computed and arranged using [Pymatgen](https://pymatgen.org/pymatgen.io.vasp.inputs.html#pymatgen.io.vasp.inputs.Kpoints.automatic_density). The geometries were considered converged when the net forces were all less than 0.03 eV/Å. Gaussian smearing of the band occupancies as applied with a smearing width of 0.01 eV. Symmetry considerations were disabled. In general, a high-spin magnetic initialization was applied with 5 µB _for d-_&#x62;lock elements (excluding Zn, Cd, Hg), _7_ µB for _f_-block elements (excluding Lu, Lr), and no magnetic character for the remaining elements. A local minimum magnetic configuration was found in each case, although there may be a lower energy global minimum for systems with complex magnetic orderings.

For additional calculation details, refer to the VASP files made available on NOMAD.
