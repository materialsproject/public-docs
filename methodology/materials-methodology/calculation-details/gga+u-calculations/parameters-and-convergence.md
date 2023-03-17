---
description: >-
  Parameter and convergence details for GGA and GGA+U calculations run by the
  Materials Project
---

# Parameters and Convergence

## Calculation Parameters

We use the Projector Augmented Wave (PAW) method for modeling core electrons with an energy cutoff of 520 eV. This cutoff corresponds to 1.3 times the highest cutoff recommended among all the pseudopotentials we use (more details can be found in the [pseudopotentials section](pseudopotentials.md)). A baseline k-point mesh of 1000/(number of atoms in the cell) is used for all computations. Specifically, the Monkhorst-Pack method is used for the k-point choices (with $$\Gamma$$-centered for hexagonal cells), and the tetrahedron method is used to perform the k-point integration. It is important to note that Pymatgen has the ability change those default parameters if they are not adequate for the computation (e.g., switch to another k-point integration scheme). Some details of our calculation method can be found in ref [\[1\]](parameters-and-convergence.md#references); however, the Materials Project has updated many parameters as documented throughout the Methodology sections. The most up-to-date input sets can be [found here](https://github.com/materialsproject/pymatgen/tree/master/pymatgen/io/vasp).

### Total energy convergence

As mentioned, we currently employ a k-point mesh of 1000 per reciprocal atom (pra). However, we have performed a convergence test of total energy with respect to k-point density and convergence energy difference for a subset of chemically diverse compounds for a previous parameter set, which employed a smaller k-point mesh of 500 pra. Using a 500 pra k-point mesh, the numerical convergence for most compounds tested was within 5 meV/atom, and 96% of compounds tested were converged to within 15 meV/atom. Results for the new parameter set will be better due to the denser k-point mesh employed. Convergence will depend on chemical system; for example, oxides were generally converged to less than 1 meV/atom. [\[2\]](parameters-and-convergence.md#references)

### Structure convergence

The energy difference for ionic convergence is set to 0.0005 \* natoms in the cell. Data on expected accuracy on cell volumes can be found in a previous paper. [\[1\]](parameters-and-convergence.md#references) We have found these parameters to yield well-converged structures in most instances; however, if the structures are to be used for further calculations that require strictly converged atomic positions and cell parameters (e.g. elastic constants, phonon modes, etc.), we recommend that users re-optimize the structures with tighter cutoffs or in force convergence mode.

## Authors

1. Shyue Ping Ong

## References

\[1]: A. Jain, G. Hautier, C. Moore, S.P. Ong, C.C. Fischer, T. Mueller, K.A. Persson, G. Ceder., A High-Throughput Infrastructure for Density Functional Theory Calculations, Computational Materials Science. vol. 50 (2011) 2295-2310.

\[2]: L. Wang, T. Maxisch, G. Ceder, Oxidation energies of transition metal oxides within the GGA+U framework, Physical Review B. 73 (2006) 1-6.
