---
description: >-
  Details of parameters for molecular DFT calculations contained in the
  Materials Project for molecules (MPcules) database.
---

# Calculation Details

For molecular properties, we use the DFT methods implemented in the Q-Chem electronic structure code. In principle, MPcules allows calculations using any level of theory (defined as the combination of exchange-correlation functional, basis set, and implicit solvent method) available in Q-Chem. In practice, the data included in MPcules is based on calculations using a small number of levels of theory. Currently, we use the range-separated hybrid generalized gradient approximation (GGA) functionals ωB97X-D\[1] and ωB97X-V\[2] as well as the range-separated hybrid meta-GGA functional wB97M-V\[3]. All calculations use the property-optimized augmented def2 basis sets from Rappoport and Furche\[4], namely def2-SVPD, def2-TZVPPD, or def2-QZVPPD. Solvent methods currently in use include vacuum (meaning that no solvent correction has been applied), the polarizable continuum model (PCM)\[5, 6], or the solvent model with density (SMD)\[7], which adds fitted terms to PCM to account for short-range interactions like the cavitation energy.

In cases where a particular property has been calculated at multiple levels of theory, we report the property calculated using the "best" level of theory available. To make this determination, we assign scores to each functional, basis set, and implicit solvent method (listed below), and we sum these scores to yield the overall level of theory score. These scores are ultimately arbitrary and are based on our subjective assessments, combined with reviewing benchmark studies in the literature. If two or more calculations use the same level of theory, the calculation with the lowest electronic energy is preferred. We note that calculations performed with different solvents cannot be compared (one solvent is not better than another, just suited for different applications), so in general, we determine the best property for each set of solvent parameters available.

* Functional scores (for currently used functoinals):
  * ωB97X-D: 5
  * ωB97X-V: 6
  * ωB97M-V: 7
* Basis set scores (for currently used basis sets):
  * def2-SVPD: 2
  * def2-TZVPPD: 6
  * def2-QZVPPD: 7
* Solvent method scores:
  * Vacuum: 1
  * PCM: 3
  * SMD: 5

Scores can also be found in Emmet:

{% embed url="https://github.com/materialsproject/emmet/blob/main/emmet-core/emmet/core/settings.py" %}

All calculations performed in MPcules are conducted on a potential energy surface (PES) at 0 K. For properties derived from vibrational frequency analyses - including infrared spectra and normal modes as well as molecular thermochemistry - the electronic energy is calculated at 0K, and all other properties assume standard state (i.e. temperature of 298.15 K and pressure of 1 atm).

Initial structures come from a variety of sources. MPcules contains molecules previously reported in the Lithium Ion Battery Electrolyte (LIBE) dataset \[8] and the MAgnesium Dataset of Electrolyte and Interphase ReAgents (MADEIRA) \[9]. In other cases, molecules from public datasets such as QM9 \[10] have been re-calculated in different levels of theory, functionalized, or otherwise modified.

## References

1. Chai, J.D. and Head-Gordon, M., 2008. Long-range corrected hybrid density functionals with damped atom–atom dispersion corrections. _Physical Chemistry Chemical Physics_, _10_(44), pp.6615-6620.
2. Mardirossian, N. and Head-Gordon, M., 2014. ωB97X-V: A 10-parameter, range-separated hybrid, generalized gradient approximation density functional with nonlocal correlation, designed by a survival-of-the-fittest strategy. _Physical Chemistry Chemical Physics_, _16_(21), pp.9904-9924.
3. Mardirossian, N. and Head-Gordon, M., 2016. ωB97M-V: A combinatorially optimized, range-separated hybrid, meta-GGA density functional with VV10 nonlocal correlation. _The Journal of chemical physics_, _144_(21).
4. Rappoport, D. and Furche, F., 2010. Property-optimized Gaussian basis sets for molecular response calculations. _The Journal of chemical physics_, _133_(13).
5. Miertuš, S., Scrocco, E. and Tomasi, J., 1981. Electrostatic interaction of a solute with a continuum. A direct utilizaion of AB initio molecular potentials for the prevision of solvent effects. _Chemical Physics_, _55_(1), pp.117-129.
6. Mennucci, B., 2012. Polarizable continuum model. _Wiley Interdisciplinary Reviews: Computational Molecular Science_, _2_(3), pp.386-404.
7. Marenich, A.V., Cramer, C.J. and Truhlar, D.G., 2009. Universal solvation model based on solute electron density and on a continuum model of the solvent defined by the bulk dielectric constant and atomic surface tensions. _The Journal of Physical Chemistry B_, _113_(18), pp.6378-6396.
8. Spotte-Smith, E.W.C., Blau, S.M., Xie, X., Patel, H.D., Wen, M., Wood, B., Dwaraknath, S. and Persson, K.A., 2021. Quantum chemical calculations of lithium-ion battery electrolyte and interphase species. _Scientific data_, _8_(1), p.203.
9. Spotte-Smith, E.W.C., Blau, S.M., Barter, D., Leon, N.J., Hahn, N.T., Redkar, N.S., Zavadil, K.R., Liao, C. and Persson, K.A., 2023. Chemical reaction networks explain gas evolution mechanisms in Mg-ion batteries. _Journal of the American Chemical Society_.
10. Ramakrishnan, R., Dral, P.O., Rupp, M. and Von Lilienfeld, O.A., 2014. Quantum chemistry structures and properties of 134 kilo molecules. _Scientific data_, _1_(1), pp.1-7.
