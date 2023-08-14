---
description: How properties related to charge transfer are determined in MPcules
---

# Redox and Electrochemical Properties

Properties related to reduction and oxidation can be calculated in two ways \[1]. In the vertical approximation, one assumes that the atomic structure of a molecule does not change upon charge transfer. We can therefore calculated vertical electron affinities (EA) and ionization energies (IE) by performing two DFT energy evaluations on the same molecular structure at two different charges. As an example, for a neutral (charge 0) molecule, the IE would be calculated by taking the difference in energy between the molecule at charge +1 and charge 0, and the EA would be calculated by taking the energy difference between the molecule at charge -1 and charge 0.

In the adiabatic approximation, one instead assumes that, upon reduction or oxidation, a molecule completely relaxes. To calculate reduction and oxidation properties in the adiabatic approximation, we compare the (free) energies of two different MPcules molcules with the same connectivity (not including metal bonds) at two different charges. It is worth noting that molecules can spontaneously decompose upon oxidation or reduction. However, as it is difficult to predict _a priori_ when such dissociative redox events will occur, we neglect these reactions. In addition to adiabatic oxidation and reduction free energies, we report reduction and oxidation potentials referenced to hte standard hydrogen electrode (SHE), using the relative potentials reported by Trasatti \[2].

## References

1. Ong, S.P. and Ceder, G., 2010. Investigation of the effect of functional group substitutions on the gas-phase electron affinities and ionization energies of room-temperature ionic liquids ions using density functional theory. _Electrochimica Acta_, _55_(11), pp.3804-3811.
2. Trasatti, S., 1986. The absolute electrode potential: an explanatory note (Recommendations 1986). _Pure and Applied Chemistry_, _58_(7), pp.955-966.
