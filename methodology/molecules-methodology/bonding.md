---
description: How chemical bonds are determined in MPcules
---

# Bonding

Especially when relying on the chemical definition of a molecule (see [Molecules Methodology - Overview](overview.md)), it is important to define the bonds in a molecule. Bonds can include covalent bonds, meaning that electrons are shared between multiple atoms, or other interactions between atoms like ionic bonds, hydrogen bonds, and coordinate bonds.

In MPcules, we currently determine molecular bonding in three ways. The simplest way relies on the [OpenBabel](http://openbabel.org/wiki/Main\_Page) cheminformatics toolkit \[1] and the [`metal_edge_extender` utility](https://pymatgen.org/pymatgen.analysis.html) defined in pymatgen. This method relies purely on valence- and distance-based heuristics, meaning that it can be used on any molecular structure, without any specific electronic structure calculations. Because of this, we rely on this OpenBabel/pymatgen method when [defining molecules](overview.md).

In addition to the purely heuristic OpenBabel/pymatgen method, we also use the method of Spotte-Smith, Blau, et al. \[2] and natural bonding orbital (NBO) analysis \[3, 4]. The Spotte-Smith-Blau method begins with the heuristic bonds defined by OpenBabel and pymatgen. Then, applying the `critic2` tool \[5] we identify additional bonds as the critical points of the optimized electron density from a DFT calculation. More specifically, if there are any critical points between atoms with a field strength greater than 0.02 (in atomic units) where the distance between atoms is < 2.5 Å, we say that those atoms are bonded.

NBO reports bonds based on electron sharing in hybrid orbitals between atoms (that is, covalent bonds). In addition to these bonds that are directly output by NBO, we can infer electrostatic bonds via orbital interactions. Specifically, to identify coordinate bonds between metals and nonmetals from NBO, we examine NBO's second-order perturbation theory analysis. If there are interactions between nonmetal lone pair orbitals and metal lone vacant or anti-Rydberg orbitals where the distance between the metal and the nonmetal is < 3.0 Å and the perturbation energy for the orbital interaction is ≥ 3.0 kcal/mol, then we say that there is a bond between the metal and the nonmetal.

In both the Spotte-Smith-Blau method based on critical point analysis and the NBO method based partially on orbital interactions, the cutoff values (in terms of interatomic distance, field strength, and perturbation energy) were determined heuristically by closely analyzing the NBO outputs for a modest, quasi-random set of molecules from MPcules.

## References

1. O'Boyle, N.M., Banck, M., James, C.A., Morley, C., Vandermeersch, T. and Hutchison, G.R., 2011. Open Babel: An open chemical toolbox. _Journal of cheminformatics_, _3_(1), pp.1-14.
2. Spotte-Smith, E.W.C., Blau, S.M., Xie, X., Patel, H.D., Wen, M., Wood, B., Dwaraknath, S. and Persson, K.A., 2021. Quantum chemical calculations of lithium-ion battery electrolyte and interphase species. _Scientific data_, _8_(1), p.203.
3. Glendening, E.D., Badenhoop, J.K., Reed, A.E., Carpenter, J.E., Bohmann, J.A., Morales, C.M., Karafiloglou, P., Landis, C.R., Weinhold, F., 2018. _NBO 7.0_. Theoretical Chemistry Institute, University of Wisconsin, Madison.
4. Glendening, E.D., Landis, C.R. and Weinhold, F., 2012. Natural bond orbital methods. _Wiley interdisciplinary reviews: computational molecular science_, _2_(1), pp.1-42.\\
5. Otero-de-la-Roza, A., Johnson, E.R. and Luaña, V., 2014. Critic2: A program for real-space analysis of quantum chemical interactions in solids. _Computer Physics Communications_, _185_(3), pp.1007-1018.
