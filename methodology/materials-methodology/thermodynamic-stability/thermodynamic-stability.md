---
description: >-
  How energy adjustments and corrections are calculated on the Materials Project
  (MP) website.
---

# Energy Corrections

## Introduction

To better model energies across diverse chemical spaces, we apply several adjustments to the total calculated energy of each material. These adjustments fall into two categories, each of which is briefly described below. Our correction scheme assumes independent, linear corrections associated with each corrected element. For example, $$\text{VO}_2$$ would receive both a '$$\text{V}$$' and an 'oxide' correction (as explained below), while elemental $$\text{V}$$ would receive no corrections. For complete details of our correction scheme, refer to Wang et al. [\[1\]](thermodynamic-stability.md#references)

## Methodology

### 1) Anion corrections

For many elements that take on negative oxidation states in solids, differences in electron localization between the elements and the solid can result in substantial errors in formation energies computed from DFT calculations. This is especially true for elements that are gaseous in their standard state - $$\text{O}_2$$, $$\text{N}_2$$, $$\text{Cl}_2$$, $$\text{F}_2$$, and $$\text{H}_2$$.

To address this, we adjust the energies of materials containing certain elements by applying a correction to anionic species, as explained in ref [\[1\]](thermodynamic-stability.md#references). Specifically, we apply energy corrections to 14 anion species -- 'oxide', 'peroxide', 'superoxide', $$\text{S}$$, $$\text{F}$$, $$\text{Cl}$$, $$\text{Br}$$, $$\text{I}$$, $$\text{N}$$, $$\text{H}$$, $$\text{Se}$$, $$\text{Si}$$, $$\text{Sb}$$, and $$\text{Te}$$. In the case of oxygen-containing compounds, separate corrections are applied to oxides, superoxides, and peroxides based on the specific bonding environment of oxygen in the material, as determined from nearest-neighbor bond lengths (e.g., <1.35 Å for superoxide, <1.49 Å for 'peroxide', and 'oxide' otherwise). Thus, $$\text{Na}_2\text{O}$$ receives an 'oxide' correction while $$\text{NaO}_2$$ receives a \`superoxide' correction.

Anion corrections are applied to a material only when it contains a corrected element _as an anion._ For example, the '$$\text{H}$$' correction is applied to $$\text{LiH}$$ but not to $$\text{H}_2\text{O}$$. A specie is classified as an anion if its estimated oxidation state (when available) is negative, or if it is the most electronegative element in the formula.

### 2) GGA / GGA+_U_ Mixing Corrections

Some compounds are better modeled with a _U_ correction term to the density functional theory Hamiltonian while others are better modeled without (i.e., regular GGA). Energies from calculations with the +_U_ correction are not directly comparable to those without. To obtain better accuracy across chemical systems, we use GGA+_U_ when appropriate, GGA otherwise, and mix energies from the two calculation methodologies by adding an energy correction term to the GGA+_U_ calculations to make them comparable to the GGA calculations.

Specifically, we use GGA+_U_ for oxide and fluoride compounds containing any of the transition metals $$\text{V}$$, $$\text{Cr}$$, $$\text{Mn}$$, $$\text{Fe}$$, $$\text{Co}$$, $$\text{Ni}$$, $$\text{W}$$, and $$\text{Mo}$$, and GGA for everything else. More details on this method can be found in refs. [\[1,2\]](thermodynamic-stability.md#references)

## Accuracy of Total Energies

To estimate the accuracy of our total energy calculations, we compute reaction data and compare against experimental data. Note that this data set was compiled using a lower k-point mesh and pseudopotentials with fewer electrons than the current Materials Project parameter set.

### Estimating errors in calculated reaction energies

The accuracy of calculated reaction energies depends on the chemical system investigated. In general, GGA calculations have similar errors among chemically similar systems. Hence, reaction energies between chemically similar systems (e.g., a reaction where the reactants and products are all oxides, such as $$\text{MgO + Al2O3} \rightarrow\text{MgAl2O4}$$ tend to have smaller errors than reactions between chemically dissimilar systems (e.g., between metals and insulators).

![](../../../.gitbook/assets/FormE\_errors.png)

_Figure 1: Errors in Calculated Formation Energies for 413 binaries in the Kubaschewski Tables. Energies are normalized to per mol atom._

To provide a quantitative indicator of the error we may expect from the reaction calculator, we have computed the reaction energies of 413 binaries in the Kubaschewski Tables formed with Group V, VI and VII anions. Figure 1 shows the errors in the calculated formation energies (compared to the experimental values) for these compounds. The mean absolute error (MAE) is around 14 kJ mol$$^{-1}$$. 75% of the calculated formation energies are within 20 kJ mol$$^{-1}$$. We also found that compounds of certain elements tend to have larger errors. For example, $$\text{Bi}$$, $$\text{Co}$$, $$\text{Pb}$$, $$\text{Eu}$$, $$\text{U}$$, $$\text{Tl}$$ and $$\text{W}$$ compounds often have errors larger than 20 kJ mol$$^{-1}$$.

It should be noted that while an MAE of 14 kJ mol$$^{-1}$$ is significantly higher than the desired chemical accuracy of 4 kJ mol$$^{-1}$$, it compares fairly well with the performance of most quantum chemistry calculations [\[3\]](thermodynamic-stability.md#references). Other than the most computationally expensive model chemistries such as G1-G3 and CBS, the reaction energy errors of most computational chemistry model chemistries are well above 10 kJ mol$$^{-1}$$.

For oxidation of the elements into binary compounds, an average error of \~4% or 33 kJ/mol-$$\text{O}_2$$ is typical.\[^9] For conventional ternary oxide formation from the elements, we have found a mean relative absolute error of about 2%. [\[4\]](thermodynamic-stability.md#references)

### Sources of error

The largest contribution to the error comes from the inability of the GGA to fully describe electronic exchange and correlation effects. In addition, there is some error associated with neglecting zero-point effects and with comparing 0K, 0atm computations with room-temperature enthalpy experiments. The latter effect was estimated to contribute less than 0.03 eV/atom by Lany. [\[5\]](thermodynamic-stability.md#references) The stability of antiferromagnetic compounds may be underestimated, as the majority of our calculations are performed ferromagnetically only. The effect of magnetism may be small (under 10 meV/atom) or large (100 meV/atom or greater), depending on the compound. For compounds with heavy elements, relativistic effects may lead to greater-than-expected errors.

### GGA errors on reaction energies between chemically similar compounds

We recently conducted a more in-depth study comparing GGA (+U) reaction energies of ternary oxides from binary oxides on 135 compounds. [\[6\]](thermodynamic-stability.md#references)

The main conclusions are:

* The error in reaction energies for the binary oxide to ternary oxides reaction energies are an order of magnitude lower than for the more often reported formation energies from the element. An error intrinsic to GGA (+U) is estimated to follow a normal distribution centered in zero (no systematic underestimation or overestimation) and with a standard deviation around 24 meV/at.
* When looking at phase stability (and for instance assessing if a phase is stable or not), the relevant reaction energies are most of the time not the formation energies from the elements but reaction energies from chemically similar compounds (e.g., two oxides forming a third oxide). Large cancellation of errors explain this observation.
* The +U is necessary for accurate description of the energetics even when reactions do not involve change in formal oxidation states

## Citations

To cite the calculation methodology, please reference the following works:

1. A. Jain, G. Hautier, C. Moore, S.P. Ong, C.C. Fischer, T. Mueller, K.A. Persson, G. Ceder., A High-Throughput Infrastructure for Density Functional Theory Calculations, Computational Materials Science, vol. 50, 2011, pp. 2295-2310. [DOI:10.1016/j.commatsci.2011.02.023](https://dx.doi.org/10.1016/j.commatsci.2011.02.023)
2. A. Jain, G. Hautier, S.P. Ong, C. Moore, C.C. Fischer, K.A. Persson, G. Ceder, Accurate Formation Enthalpies by Mixing GGA and GGA+U calculations, Physical Review B, vol. 84, 2011, p. 045115. [DOI:10.1103/PhysRevB.84.045115](https://doi.org/10.1103/PhysRevB.84.045115)

## References

\[1]: Wang, A., Kingsbury, R.S., Horton, M., Jain, A., Ong, S.P., Dwaraknath, S., Persson, K. A framework for quantifying uncertainty in DFT energy corrections. _Scientific Reports_ 11 (2021), 15496. [DOI: 10.1038/s41598-021-94550-5](https://doi.org/10.1038/s41598-021-94550-5)

\[2]: A. Jain, G. Hautier, S.P. Ong, C. Moore, C.C. Fischer, K.A. Persson, G. Ceder, Formation Enthalpies by Mixing GGA and GGA+U calculations, Physical Review B, vol. 84 (2011), 045115.

\[3]: J.B. Foresman, A.E. Frisch, Exploring Chemistry With Electronic Structure Methods: A Guide to Using Gaussian, Gaussian. (1996).

\[4]: A. Jain, S.-a Seyed-Reihani, C.C. Fischer, D.J. Couling, G. Ceder, W.H. Green, Ab initio screening of metal sorbents for elemental mercury capture in syngas streams, Chemical Engineering Science. 65 (2010) 3025-3033.

\[5]: S. Lany, Semiconductor thermochemistry in density functional calculations, Physical Review B. 78 (2008) 1-8.

\[6]: G. Hautier, S.P. Ong, A. Jain, C. J. Moore, G. Ceder, Accuracy of density functional theory in predicting formation energies of ternary oxides from binary oxides and its implication on phase stability, Physical Review B, 85 (2012), 155208



## Acknowledgments

Thank you to the original authors of this page:

1. Anubhav Jain
2. Shyue Ping Ong
3. Geoffroy Hautier
4. Charles Moore
