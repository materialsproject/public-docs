---
description: How MPcules collects data from natural bonding orbital (NBO) analysis
---

# Natural Atomic and Molecular Orbitals

NBO\[1,2] processes and analyzes the optimized wavefunction produced by a DFT calculation. First, the atom-centered (typically Gaussian) basis set is converted into a basis of natural atomic orbitals (e.g. s, p, d, and f). These natural atomic orbitals are then used to construct various hybrid orbitals, including natural hybrid orbitals, natural bond orbitals, and natural localized molecular orbitals. From these, NBO can report detailed information regarding atomic populations, lone pairs, bonds, and interactions between different orbitals.

Currently, MPcules reports NBO atomic populations (including the total number of electrons on an atom, the number of core, valence, and Rydberg electrons), lone pair and bond information (including the fraction of the hybrid orbital made up of different types of natural atomic orbitals, as well as its total occupancy), and the output of second-order perturbation theory analysis of donor-acceptor orbital interactions (including the perturbation energy, the energy difference between donor and acceptor, and the Fock matrix element for the interaction). Where appropriate, we also report orbital types, using NBO's internal code. For instance, bonding orbitals are labeled "BD", antibonding orbitals are "BD\*", lone pairs are "LP", and Rydberg orbitals are "RY".

For open-shell molecules, NBO performs separate analyses on the ɑ and β electrons. Accordingly, orbital information in MPcules is structured differently for closed-shell and open-shell molecules.

## References

1. Glendening, E.D., Badenhoop, J.K., Reed, A.E., Carpenter, J.E., Bohmann, J.A., Morales, C.M., Karafiloglou, P., Landis, C.R., Weinhold, F., 2018. _NBO 7.0_. Theoretical Chemistry Institute, University of Wisconsin, Madison.
2. Glendening, E.D., Landis, C.R. and Weinhold, F., 2012. Natural bond orbital methods. _Wiley interdisciplinary reviews: computational molecular science_, _2_(1), pp.1-42.

