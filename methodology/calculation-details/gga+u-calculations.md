---
description: >-
  Description of the Hubbard U terms used in the density functional theory (DFT)
  calculations contained in the Materials Project (MP) database.
---

# GGA+U Calculations

It is well-known that first principles calculations within the local density approximation (LDA) or generalized gradient approximation (GGA) lead to considerable error in calculated redox reaction energies of many transition metal compounds. This error arises from the self-interaction error in LDA and GGA, which is not canceled out in redox reactions where an electron is transferred between significantly different environments, such as between a metal and a transition metal or between a transition metal and oxygen or fluorine. Extensive discussion of this issue can be found in the following works. [\[1-4\]](gga+u-calculations.md#references)

In the Materials Project, we have calibrated $$U$$ values for many transition metals of interest using the approach outlined in Wang et al.'s work [\[5\]](gga+u-calculations.md#references). At the present moment, $$U$$ values have only been calibrated for transition metal oxide systems. $$U$$ values were calibrated for the following elements: $$\text{Co}$$, $$\text{Cr}$$, $$\text{Fe}$$, $$\text{Mn}$$, $$\text{Mo}$$, $$\text{Ni}$$, $$\text{V}$$ and $$\text{W}$$. The choice of systems to which we apply $$U$$ was largely determined by our experience and by systematic benchmarking. It is very likely that we will expand calibration of $$U$$ values to more chemical systems in the future.

In the Materials Project, for an oxide or fluoride material with a transition element listed previously, with the VASP input settings constructed according to the logic defined in [pymatgen](https://github.com/materialsproject/pymatgen/blob/master/pymatgen/io/vasp/MPRelaxSet.yaml).

Note that for fluorides, the $$U$$ value gets set to the one calibrated from the oxide system, although in principle our architecture allows different $$U$$ values to be set for oxides and fluorides respectively.

## Calibration of U values

The $$U$$ values were obtained by fitting to experimental binary formation enthalpies as described in Wang et al.'s work. This method is simple yet accurately reproduces phase stabilities. A least squares method of obtaining the correct $$U$$ value was used, as follows:

1. For each non-overlapping formation energy reaction considered, we find the region where the formation energy error passes zero. For the $$\text{V-O}$$ system, this includes the following:
   * $$2\text{V}_2\text{O}_3 + \text{O}_2 \rightarrow 4 \text{VO}_2$$
   * $$4 \text{VO}_2 + \text{O}_2 \rightarrow 2\text{V}_2\text{O}_5$$
2. For each formation energy region identified, we fit the linear equation $$\begin{align} \mbox{Error/redox} & = m U + c \end{align}$$ to the final $$U$$ range. In the case of $$\text{V}$$, we will have two sets of $$(m,c)$$.
3. We find the _U_ value that minimizes the sum of square Error / Redox.
4. In the case of $$\text{V}$$, we get a $$U$$ value of 3.25.

## U values

The full list of U values used is described in the table below. For oxides and fluorides containing any of the elements, only GGA+U calculations are performed.

| Element | System | Fitting Reaction                                                                                                                                                                                    | Redox Couple                                                                                                                                            | Calibrated U (eV) | Comments                                                                                                                                                                        |
| ------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Co      | Oxides | $$6\text{CoO} + \text{O}_2 \rightarrow 2 \text{Co}_3\text{O}_4$$                                                                                                                                    | $$\text{Co}^{2+} \rightarrow\text{Co}^{2.67+}$$                                                                                                         | 3.32              |                                                                                                                                                                                 |
| Cr      | Oxides | $$2/3\text{Cr}_2 \text{O}_3 + \text{O}_2 \rightarrow 4/3 \text{CrO}_3$$                                                                                                                             | $$\text{Cr}^{3+} \rightarrow \text{Cr}^{6+}$$                                                                                                           | 3.7               |                                                                                                                                                                                 |
| Fe      | Oxides | <p><span class="math">6\text{FeO} + \text{O}_2 \rightarrow 2 \text{Fe}_3 \text{O}_4</span><br><span class="math">4\text{Fe}_3\text{O}_4 +\text{O}_2 \rightarrow 6 \text{Fe}_2 \text{O}_3</span></p> | <p><span class="math">\text{Fe}^{2+} \rightarrow \text{Fe}^{2.67+}</span><br><span class="math">\text{Fe}^{2.67+} \rightarrow \text{Fe}^{3+}</span></p> | 5.3               |                                                                                                                                                                                 |
| Mn      | Oxides | <p><span class="math">6 \text{MnO} + \text{O}_2 \rightarrow 2 \text{Mn}_3\text{O}_4</span><br><span class="math">\text{Mn}_3\text{O}_4   + \text{O}_2\rightarrow 3 \text{MnO}_2</span></p>          | <p><span class="math">\text{Mn}^{2+} \rightarrow \text{Mn}^{2.67+}</span><br><span class="math">\text{Mn}^{2.67+} \rightarrow \text{Mn}^{4+}</span></p> | 3.9               | $$\text{Mn}_2\text{O}_3$$ was explicitly excluded from calibration set due to the large number of atoms in its unit cell.                                                       |
| Mo      | Oxides | $$2 \text{MoO}_2 + \text{O}_2 \rightarrow 2 \text{MnO}_3$$                                                                                                                                          | $$\text{Mo}^{4+} \rightarrow \text{Mo}^{6+}$$                                                                                                           | 4.38              |                                                                                                                                                                                 |
| Ni      | Oxides | $$\text{Li}_2 \text{O} + 2\text{NiO} + 1/2 \text{O}_2 \rightarrow 2 \text{LiNiO}_2$$                                                                                                                | $$\text{Ni}^{2+} \rightarrow \text{Ni}^{3+}$$                                                                                                           | 6.2               | Binary formation energies are not readily available for Ni. The Ni U calibration was performed using a ternary oxide formation energy.[\[5\]](gga+u-calculations.md#references) |
| V       | Oxides | <p><span class="math">2 \text{V}_2 \text{O}_3 + \text{O}_2 \rightarrow 4 \text{VO}_2</span><br><span class="math">4 \text{VO}_2 + \text{O}_2 \rightarrow 2 \text{V}_2 \text{O}_5</span></p>         | <p><span class="math">\text{V}^{3+} \rightarrow \text{V}^{4+}</span><br><span class="math">\text{V}^{4+} \rightarrow \text{V}^{5+}</span></p>           | 3.25              | $$\text{VO}$$ was explicitly excluded from calibration due to its known metallic nature.                                                                                        |
| W       | Oxides | $$2 \text{WO}_2 + \text{O}_2 \rightarrow 2 \text{WO}_3$$                                                                                                                                            | $$\text{W}^{4+} \rightarrow \text{W}^{6+}$$                                                                                                             | 6.2               |                                                                                                                                                                                 |

## Caveats

The U values are calibrated for phase stability analyses, and should be used with care if applied to obtain other properties such as band structures. Also, the U values depend on the pseudopotential used. Further, typically, U values should be site specific, however in our approach, U values were applied to all sites with an element listed above, and only to the d-orbitals. A discussion of the pseudopotentials used in the Materials Project can be found here.

## Authors

1. Shyue Ping Ong

## References

\[1]: F. Zhou, M. Cococcioni, C. A. Marianetti, D. Morgan and G. Ceder. First-principles prediction of redox potentials in transition-metal compounds with LDA+U. Physical Review B, 2004, 70, 235121. [doi:10.1103/PhysRevB.70.235121](doi:10.1103/PhysRevB.70.235121)

\[2]: M. Cococcioni, S. de Gironcoli, Linear response approach to the calculation of the effective interaction parameters in the LDA+U method. Physical Review B, 2005, 71, 035105. [doi:10.1103/PhysRevB.71.035105](doi:10.1103/PhysRevB.71.035105)

\[3]: L. Wang, T. Maxisch, & G. Ceder. Oxidation energies of transition metal oxides within the GGA+U framework. Physical Review B. 2006, 73, 195107, [doi:10.1103/PhysRevB.73.195107](doi:10.1103/PhysRevB.73.195107)

\[4]: A. Jain, G. Hautier, S. P. Ong, C. Moore, C. Fischer, K. A. Persson, & G. Ceder. Formation enthalpies by mixing GGA and GGA + U calculations. Physical Review B, 2011, 84(4), 045115. [doi:10.1103/PhysRevB.84.045115](doi:10.1103/PhysRevB.84.045115)

\[5]: M. Wang, A. Navrotsky Enthalpy of formation of LiNiO2, LiCoO2 and their solid solution, LiNi1-xCoxO2, Solid State Ionics, vol. 166, no. 1-2, pp. 167-173, Jan. 2004.
