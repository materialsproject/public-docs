---
description: An overview of the molecules methodology
---

# Overview

While the Materials Project has historically focused on materials, we also calculate the properties of small molecules. The term "small molecules"  is somewhat vague but typically refers to molecules with molecular weight below 1000 atomic mass units or amu (for reference, the molecular weight of water is 18 amu). In practice, we use the term "small molecule" to distinguish from polymers and biomolecules (like proteins).

## What Is a Molecule?

A "molecule" is typically defined as two or more atoms that are chemically bound. When we use the term "molecule", we also include single atoms (e.g. the hydrogen atom, H) and monatomic ions (e.g. fluoride, F-), because these species can be important for calculating certain properties like metal binding energies.

Molecules are distinguished on the basis of their chemical formulas, charge, and spin multiplicities. For instance, we could write "3O2" to refer to neutral diatomic oxygen (O2) in the triplet ground state. Beyond this simple definition, one can either distinguish between molecules using the idea of potential energy surfaces (PES) or else using the idea of chemical bonding. 

If a molecule is defined as a local minimum on a PES (the physical definition of a molecule), then every unique PES minimum obtained by a geometry optimization calculation (in terms of interatomic distances, angles, dihredrals, etc.) is a distinct molecule. It is worth noting that this physical definition is used by the Materials Project to differentiate materials.

In contrast, the chemical definition says molecules are distinguished by the different ways that atoms are connected by chemical bonds and interatomic interactions. In many cases, different minima on the PES have the same bonding structure and only differ by e.g. bond rotations. These conformational isomers or conformers are typically viewed as representing the same molecule, and most chemical observables (like vibrational spectra and electrochemical properties) are averaged over different interconverting conformers. The chemical definition is more complex than the physical picture because it requires additional definitions - i.e., what is a "bond"?

In MPcules, we use both the physical and the chemical definitions, but for most purposes, we rely on the chemical definition based on bonding.

## New vs. Legacy Data

The original molecule dataset included in the Materials Project, developed through the Electrolyte Genome project as part of the Joint Center for Energy Storage Research (JCESR), was focused on developing next-generation electrolytes for batteries. As such, the Electrolyte Genome and the original Molecules Explorer were narrowly focused on molecular electrochemical properties.

We have since expanded our molecular dataset, considering a larger set of molecules and a more diverse set of properties - not just electrochemical, but thermodynamic, electronic, vibrational, and more. Here, we primarily describe this new database, which we call the Materials Project for Molecules or "MPcules". This section mainly describes the methods used to generate the MPcules database. For further details regarding MPcules, please see our recent publication:\[1]

{% embed url="https://doi.org/10.1039/D3DD00153A" %}

For information about the Electrolyte Genome project and the legacy molecules data on the Materials Project, see \[2] and \[3].

## References:

1. Spotte-Smith, E.W.C., Cohen, O.A., Blau, S.M., Munro, J.M., Yang, R., Guha, R.D., Patel, H.D., Vijay, S., Huck, P., Kingsbury, R., Horton, M.K., Persson, K.A., 2023. A database of molecular properties integrated in the Materials Project. _Digital Discovery_.
2. Qu, X., Jain, A., Rajput, N.N., Cheng, L., Zhang, Y., Ong, S.P., Brafman, M., Maginn, E., Curtiss, L.A. and Persson, K.A., 2015. The Electrolyte Genome project: A big data approach in battery materials discovery. _Computational Materials Science_, _103_, pp.56-67.
3. Cheng, L., Assary, R.S., Qu, X., Jain, A., Ong, S.P., Rajput, N.N., Persson, K. and Curtiss, L.A., 2015. Accelerating electrolyte discovery for energy storage with high-throughput screening. _The journal of physical chemistry letters_, _6_(2), pp.283-291.
