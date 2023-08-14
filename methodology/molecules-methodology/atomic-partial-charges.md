---
description: How partial charges are determined in MPcules
---

# Atomic Partial Charges

Partial charges can be approximated from DFT calculations using a variety of methods, including calculating the population of atomic and molecular orbitals, partitioning the electron density around a molecule into atomic regions, or calculation an electrostatic potential. We currently include atomic partial charges calculated using four methods: Mulliken population analysis \[1], the restrained electrostatic potential (RESP) \[2], Bader charges \[3], and natural atomic populations from the Natural Bond Orbital (NBO) program \[4, 5]

We note that different methods of partial charge approximation can differ both quantitatively and qualitatively. In particular, the Mulliken method is has been reported to behave poorly, in part due to a strong dependence on the basis set used for the DFT calculation. When available, we recommend the use of NBO charges, and specifically advise against using Mulliken charges when multiple options are available.

## References

1. Mulliken, R.S., 1955. Electronic population analysis on LCAO–MO molecular wave functions. I. _The Journal of chemical physics_, _23_(10), pp.1833-1840.
2. Bayly, C.I., Cieplak, P., Cornell, W. and Kollman, P.A., 1993. A well-behaved electrostatic potential based method using charge restraints for deriving atomic charges: the RESP model. _The Journal of Physical Chemistry_, _97_(40), pp.10269-10280.
3. Bader, R.F.W., 1990. _Atoms in Molecules: A Quantum Theory_. Clarendon Press.
4. Glendening, E.D., Badenhoop, J.K., Reed, A.E., Carpenter, J.E., Bohmann, J.A., Morales, C.M., Karafiloglou, P., Landis, C.R., Weinhold, F., 2018. _NBO 7.0_. Theoretical Chemistry Institute, University of Wisconsin, Madison.
5. Glendening, E.D., Landis, C.R. and Weinhold, F., 2012. Natural bond orbital methods. _Wiley interdisciplinary reviews: computational molecular science_, _2_(1), pp.1-42.
