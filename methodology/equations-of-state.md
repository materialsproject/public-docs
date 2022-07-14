---
description: >-
  How equations of state (EOS) are calculated on the Materials Project (MP)
  website.
---

# Equations of State

## Introduction

Thermodynamic equations of state (EOS) for crystalline solids describe material behaviors under changes in pressure, volume, entropy and temperature. Despite over a century of theoretical development and experimental testing of energy-volume (E-V) EOS for solids, there is still a lack of consensus with regard to which equation is optimal, as well as to what metrics are most appropriate for making this judgment.

Calculation of EOS is automated using self-documenting workflows compiled in the atomate code base. Atomate couples pymatgen for materials analysis, custodian for just-in-time debugging of DFT codes, and Fireworks for workflow management. The EOS workflow begins with a structure optimization and subsequently calculates the energy of isotropic deformations including ionic relaxation with volumetric strain ranging from -15.7% to 15.7% (-5% to 5% linear strain) of the optimized structure. Density-functional-theory (DFT) calculations were performed as necessary using the projector augmented wave (PAW) method as implemented in the Vienna Ab Initio Simulation Package (VASP) within the Perdew-Burke-Enzerhof (PBE) Generalized Gradient Approximation (GGA) formulation of the exchange-correlation functional. A cut-off for the plane waves of 520 eV is used and a uniform k-point density of approximately 1,000 per reciprocal atom is employed. In addition, standard Materials Project Hubbard U corrections are used for a number of transition metal oxides, as documented and implemented in the pymatgen VASP input sets. We note that the computational and convergence parameters were chosen consistently with the settings used in the Materials Project to enable direct comparisons with the large set of available MP data.

## Fitted Equation Forms

| **Equation**      | $$\boldsymbol{E(\nu^*)}$$                                                                                                                    | $$\boldsymbol{K(\nu = 1)}$$ | $$\boldsymbol{K'(\nu = 1)}$$ | **Ref**                                   |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- | ---------------------------- | ----------------------------------------- |
| Birch (Euler)     | $$E = E_o^{**} + BV_o\Big(\big(\nu^{-\frac{2}{3}} - 1\big)^2 + \frac{C}{2}\big(\nu^{-\frac{2}{3}} - 1\big)^3\Big)$$                          | $$\frac{8B}{9}$$            | $$C + 4$$                    | [\[1\]](equations-of-state.md#references) |
| Birch (Lagrange)  | $$E = E_o + BV_oC - BV_o\nu^{\frac{2}{3}}\Big(\big(C - 2\big)\big(1 - \nu^{\frac{2}{3}}\big)^2 + C\big(1 - \nu^{\frac{2}{3}}\big) + C\Big)$$ | $$\frac{16B}{9}$$           | $$C - 2$$                    | [\[1\]](equations-of-state.md#references) |
| Mie-Gruneisen     | $$E = E_o + \frac{BV_o}{C} - \frac{BV_o}{C - 1}\Big(\nu^{-\frac{1}{3}} - \frac{1}{C}\nu^{-\frac{C}{3}}\Big)$$                                | $$\frac{B}{9}$$             | $$\frac{7 + C}{3}$$          | [\[2\]](equations-of-state.md#references) |
| Murnaghan         | $$E = E_o + \frac{BV_o}{(C + 1)}\Big(\frac{\nu^{-C} - 1}{C} + \nu - 1\Big)$$                                                                 | $$B$$                       | $$C + 1$$                    | [\[3\]](equations-of-state.md#references) |
| Pack-Evans-James  | $$E = E_o + \frac{BV_o}{C}\Big(\frac{1}{C}\big(e^{3C(1 - \nu^{\frac{1}{3}})} - 1\big) - 3\big(1 -\nu^{\frac{1}{3}}\big)\Big)$$               | $$B$$                       | $$C + 1$$                    | [\[4\]](equations-of-state.md#references) |
| Poirier-Tarantola | $$E = E_o + BV_o\Big(ln(\nu)\Big)^2\Big(3 - C\big(ln(\nu)\big)\Big)$$                                                                        | $$6B$$                      | $$C + 2$$                    | [\[5\]](equations-of-state.md#references) |
| Tait              | $$E = E_o + \frac{BV_o}{C}\Big(\nu - 1 + \frac{1}{C}\big(e^{C(1 -\nu)} - 1\big)\Big)$$                                                       | $$B$$                       | $$C - 1$$                    | [\[6\]](equations-of-state.md#references) |
| Vinet             | $$E = E_o + \frac{BV_o}{C^2}\Big(1 - \big(1 + C(\nu^{\frac{1}{3}} - 1)\big)e^{-C(\nu^{\frac{1}{3}} - 1)}\Big)$$                              | $$\frac{B}{9}$$             | $$\frac{2}{3}C + 1$$         | [\[7\]](equations-of-state.md#references) |

$$\nu^* = \frac{V}{V_o}$$, where $$V_o$$ is the volume at zero pressure.

$$E_o^{**} = E(\nu = 1)$$

## Citation

To cite the EOS data in the Materials Project, please reference the following work:

Latimer, K., Dwaraknath, S., Mathew, K., Winston, D., Persson, K. A. Evaluation of thermodynamic equations of state across chemistry and structure in the materials project. _NPJ Computational Materials._ **4,** 1, 2057-3960 (2018). [DOI:10.1038/s41524-018-0091-x](https://doi.org/10.1038/s41524-018-0091-x)

## Authors

1. Katherine Latimer
2. Shyam Dwaraknath
3. Donny Winston

## References

\[1]: Birch, F. Finite elastic strain of cubic crystals. _Physical Review._ **71,** 11, 809–824 (1947).

\[2]: Roy, B. and Roy, S. B. Applicability of isothermal three-parameter equations of state of solids: A reappraisal. _Journal of Physics: Condensed Matter._ **17,** 39, 6193–6216 (2005).

\[3]: Murnaghan, F. D. The compressibility of media under extreme pressures. _Proceedings of the National Academy of Sciences._ **30,** 244–247 (1944).

\[4]: Pack, D., Evans, W., James, H. The Propagation of Shock Waves in Steel and Lead. _The Proceedings of the Physical Society._ **60,** 1–8 (1948).

\[5]: Poirier,  J. P. and Tarantola, A. A logarithmic equation of state. _Physics of the Earth and Planetary Interiors._ **109,** 1-2, 1–8 (1998).

\[6]: Dymond,  J. H. and Malhotra, R. The Tait equation: 100 years on. _International Journal of Thermophysics._ **9,** 6, 941–951 (1988).

\[7]: Vinet, P., Ferrante, J., Rose,  J. H., Smith,  J. R. Compressibility of solids. _Journal of Geophysical Research._ **92,** 9319–9325 (1987).
