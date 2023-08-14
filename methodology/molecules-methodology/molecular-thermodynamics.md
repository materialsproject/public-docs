---
description: How MPcules calculate the thermochemical properties of molecules
---

# Molecular Thermodynamics

DFT SCF calculations produce an electronic energy as output. This can be used to determine the relative stability of different structures and calculate reaction energies. If one performs a vibrational frequency analysis, one can instead calculate the enthalpy (including the zero-point vibrational energy) or the Gibbs free energy, which are more natural quantities for comparison to experiments.

To calculate free energies at reduced cost, computational chemists often perform geometry optimization and vibrational frequency analyses using relatively inexpensive levels of theory (e.g. using a small basis set, or ignoring solvent effects) and then re-calculate the electronic energy using a more accurate and expensive level of theory (e.g. using a larger basis set or including an implicit solvent model). We can calculate the molecular thermodynamics using two methods: one in which all thermodynamic quantities of interest (e.g. electronic energy, enthalpy, and Gibbs free energy) are calculated from a single vibrational frequency analysis calculation, and another in which most properties are obtained from a vibrational frequency analysis and the electronic energy is obtained from a single-point energy calculation performed on the same structure at a higher level of theory.

In MPcules, we consider both of these approaches. If it is possible to calculate a molecule's thermodynamic properties both with and without a single-point energy corrections, then the scores (see [Calculation Details](calculation-details.md)) for the best uncorrected document and best corrected document are compared. For the corrected document, we average the scores for the vibrational frequency analysis and the single-point correction.
