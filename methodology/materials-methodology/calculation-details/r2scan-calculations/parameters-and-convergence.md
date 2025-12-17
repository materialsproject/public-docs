---
description: >-
  Parameter and convergence details for r2SCAN calculations run by the Materials
  Project
---

# Parameters and Convergence

## Calculation Parameters

We use the projector-augmented wave (PAW) or modeling core electrons with an energy cutoff of 680 eV. K-point grids were generated automatically by VASP using KSPACING values ranging from 0.22/Å to 0.44/Å. Specifically, the Monkhorst-Pack method is used for grid generation (with $$\Gamma$$-centered for hexagonal cells), and the tetrahedron method is used to perform the k-point integrations. These were determined from the GGA-estimated bandgap of each material based on the work by Wisesa et al. [\[1\]](parameters-and-convergence.md#references). More details regarding the calculation method can be found in ref [\[2\]](parameters-and-convergence.md#references); however, the Materials Project has updated many parameters as documented throughout the Methodology sections. The most up-to-date input sets can be [found here](https://github.com/materialsproject/pymatgen/blob/f9d9fe8e0ce09ef30cc03bcc4e9937d27afd5a6a/src/pymatgen/io/vasp/sets.py#L1315).

### Convergence

Plane-wave energy cutoff and k-point density settings were selected such that formation energies converged within approximately 1 meV/atom for a benchmark set of 21 materials and were selected to be conservatively high [\[2\]](parameters-and-convergence.md#references): 

| Formula  | Spacegroup | Materials Project ID |
| -------- | ---------- | -------------------- |
| AlN      | P63mc      | mp-661               |
| Al2O3    | R3c        | mp-1143              |
| BN       | P63/mmc    | mp-984               |
| BaBeSiO4 | Cm         | mp-550751            |
| CeO2     | Fm3m       | mp-20194             |
| CaF2     | Fm3m       | mp-2741              |
| EuO      | Fm3m       | mp-21394             |
| FeP      | Pnma       | mp-1005              |
| FeS      | P4/nmm     | mp-505531            |
| GaAs     | F43m       | mp-2534              |
| InSb     | F43m       | mp-20012             |
| LiH      | Fm3m       | mp-23703             |
| LiF      | Fm3m       | mp-1138              |
| LiCl     | P63mc      | mp-1185319           |
| Li2O     | Fm3m       | mp-1960              |
| LiN      | I4m2       | mp-1059612           |
| MoS2     | P3m1       | mp-1027525           |
| NaI      | Fm3m       | mp-23268             |
| SrI2     | Pnma       | mp-568284            |
| TiO2     | C2/m       | mp-554278            |
| VO2      | P21/c      | mp-1102963           |

## References

\[1] P. Wisesa, K. A. McGill, and T. Mueller, Efficient generation of generalized Monkhorst-Pack grids through the use of informatics, Phys. Rev. B 93, 1 (2016).

\[2] R. Kingsbury, A. S. Gupta, C. J. Bartel, J. M. Munro, S. Dwaraknath, M. Horton, and K. A. Persson Phys. Rev. Materials 6, 013801 (2022)
