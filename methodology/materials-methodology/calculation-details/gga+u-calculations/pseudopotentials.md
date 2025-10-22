---
description: >-
  Description of the pseudo-potentials (PSP) used in the GGA and GGA+U
  calculations.
---

# Pseudo-potentials

{% hint style="info" %}
On 2023-05-02, we changed the Yb PSP in _all_ VASP input sets from `Yb_2` to `Yb_3` as `Yb_2` gives incorrect thermodynamics for most systems with Yb3+. See [pymatgen#2968](https://github.com/materialsproject/pymatgen/issues/2968) for details. We are also recomputing all Yb compounds in MP for an upcoming database release. The release notes will highlight this change.
{% endhint %}

Pseudopotentials are used to reduce computation time by replacing the full electron system in the Coulombic potential by a system only taking explicitly into account the "valence" electrons (i.e., the electrons participating into bonding) but in a pseudopotential. This approach not only reduces the electron number but also the energy cutoff necessary (this is critical in plane-wave-based computations). All computations in the materials project have been performed using a specific type of very efficient pseudopotentials: the projector augmented wave (PAW) pseudopotentials. [\[1\]](pseudopotentials.md#references) We used the library of PAW pseudopotentials provided by VASP but for a given element there are often several possibilities in the VASP library. This wiki presents how the choices between the different pseudopotential options were made.

## The strategy

As a test set, we ran all elements and binary oxides present in the ICSD with the available PAW pseudopotentials. As it is difficult to test for all properties (structural, electronic, etc...), we chose to be inclusive and to select the pseudopotential with the largest number of electrons (high e) **except** if convergence issues were seen on our test set, or if previous experience excluded a specific pseudopotential. We also excluded pseudopotentials with too large an energy cutoff.

We also compared to recommendations from the VASP manual present in [1](https://www.vasp.at/wiki/index.php/Available_PAW_potentials).

Finally, as we had energies for elements and binary oxides, we compared binary oxide formation energies with the available pseudopotentials. The oxygen molecule energy was obtained from Wang et al. Please note that this data is pure GGA and some chemistries (e.g., transition metals) will give extremely bad formation energy results in GGA. This is not an issue with the pseudopotential but with the functional, so we do not focus on that issue in this wiki.

## Pseudopotential comments and choice

### Summary

The PBE GGA and PBE+U calculations in the Materials Project use older pseudopotentials, some of which are no longer distributed by VASP. In particular, the Ce, Eu, Gd, Ge, Li, Mg, and Na POTCARs come from a pre-March 2002 release, and are no longer distributed by VASP. The rest of the POTCARs listed below can be approximated by the POTCARs in[ the PBE 2010 release](https://vasp.at/wiki/Available_pseudopotentials#LDA_\(2010\),_PW91_\(2006\)_and_PBE_\(2010\)_PAW_potentials), which is still distributed by VASP.

These POTCARs are selected by the [MPRelaxSet](https://github.com/materialsproject/pymatgen/blob/821893487db356bc4d2b5414de592c2abc32a261/src/pymatgen/io/vasp/MPRelaxSet.yaml#L84) in pymatgen.

| Element | TITEL                     |
| ------- | ------------------------- |
| Ac      | PAW\_PBE Ac 06Sep2000     |
| Ag      | PAW\_PBE Ag 06Sep2000     |
| Al      | PAW\_PBE Al 04Jan2001     |
| Ar      | PAW\_PBE Ar 07Sep2000     |
| As      | PAW\_PBE As 06Sep2000     |
| Au      | PAW\_PBE Au 06Sep2000     |
| B       | PAW\_PBE B 06Sep2000      |
| Ba      | PAW\_PBE Ba\_sv 06Sep2000 |
| Be      | PAW\_PBE Be\_sv 06Sep2000 |
| Bi      | PAW\_PBE Bi 08Apr2002     |
| Br      | PAW\_PBE Br 06Sep2000     |
| C       | PAW\_PBE C 08Apr2002      |
| Ca      | PAW\_PBE Ca\_sv 06Sep2000 |
| Cd      | PAW\_PBE Cd 06Sep2000     |
| Ce      | PAW\_PBE Ce 28Sep2000     |
| Cl      | PAW\_PBE Cl 17Jan2003     |
| Co      | PAW\_PBE Co 06Sep2000     |
| Cr      | PAW\_PBE Cr\_pv 07Sep2000 |
| Cs      | PAW\_PBE Cs\_sv 08Apr2002 |
| Cu      | PAW\_PBE Cu\_pv 06Sep2000 |
| Dy      | PAW\_PBE Dy\_3 06Sep2000  |
| Er      | PAW\_PBE Er\_3 06Sep2000  |
| Eu      | PAW\_PBE Eu 08Apr2002     |
| F       | PAW\_PBE F 08Apr2002      |
| Fe      | PAW\_PBE Fe\_pv 06Sep2000 |
| Ga      | PAW\_PBE Ga\_d 06Sep2000  |
| Gd      | PAW\_PBE Gd 08Apr2002     |
| Ge      | PAW\_PBE Ge\_d 06Sep2000  |
| H       | PAW\_PBE H 15Jun2001      |
| He      | PAW\_PBE He 05Jan2001     |
| Hf      | PAW\_PBE Hf\_pv 06Sep2000 |
| Hg      | PAW\_PBE Hg 06Sep2000     |
| Ho      | PAW\_PBE Ho\_3 06Sep2000  |
| I       | PAW\_PBE I 08Apr2002      |
| In      | PAW\_PBE In\_d 06Sep2000  |
| Ir      | PAW\_PBE Ir 06Sep2000     |
| K       | PAW\_PBE K\_sv 06Sep2000  |
| Kr      | PAW\_PBE Kr 07Sep2000     |
| La      | PAW\_PBE La 06Sep2000     |
| Li      | PAW\_PBE Li\_sv 23Jan2001 |
| Lu      | PAW\_PBE Lu\_3 06Sep2000  |
| Mg      | PAW\_PBE Mg\_pv 06Sep2000 |
| Mn      | PAW\_PBE Mn\_pv 07Sep2000 |
| Mo      | PAW\_PBE Mo\_pv 08Apr2002 |
| N       | PAW\_PBE N 08Apr2002      |
| Na      | PAW\_PBE Na\_pv 05Jan2001 |
| Nb      | PAW\_PBE Nb\_pv 08Apr2002 |
| Nd      | PAW\_PBE Nd\_3 06Sep2000  |
| Ne      | PAW\_PBE Ne 05Jan2001     |
| Ni      | PAW\_PBE Ni\_pv 06Sep2000 |
| Np      | PAW\_PBE Np 06Sep2000     |
| O       | PAW\_PBE O 08Apr2002      |
| Os      | PAW\_PBE Os\_pv 20Jan2003 |
| P       | PAW\_PBE P 17Jan2003      |
| Pa      | PAW\_PBE Pa 07Sep2000     |
| Pb      | PAW\_PBE Pb\_d 06Sep2000  |
| Pd      | PAW\_PBE Pd 05Jan2001     |
| Pm      | PAW\_PBE Pm\_3 07Sep2000  |
| Pr      | PAW\_PBE Pr\_3 07Sep2000  |
| Pt      | PAW\_PBE Pt 05Jan2001     |
| Pu      | PAW\_PBE Pu 06Sep2000     |
| Rb      | PAW\_PBE Rb\_sv 06Sep2000 |
| Re      | PAW\_PBE Re\_pv 06Sep2000 |
| Rh      | PAW\_PBE Rh\_pv 06Sep2000 |
| Ru      | PAW\_PBE Ru\_pv 06Sep2000 |
| S       | PAW\_PBE S 17Jan2003      |
| Sb      | PAW\_PBE Sb 06Sep2000     |
| Sc      | PAW\_PBE Sc\_sv 07Sep2000 |
| Se      | PAW\_PBE Se 06Sep2000     |
| Si      | PAW\_PBE Si 05Jan2001     |
| Sm      | PAW\_PBE Sm\_3 07Sep2000  |
| Sn      | PAW\_PBE Sn\_d 06Sep2000  |
| Sr      | PAW\_PBE Sr\_sv 07Sep2000 |
| Ta      | PAW\_PBE Ta\_pv 07Sep2000 |
| Tb      | PAW\_PBE Tb\_3 06Sep2000  |
| Tc      | PAW\_PBE Tc\_pv 06Sep2000 |
| Te      | PAW\_PBE Te 08Apr2002     |
| Th      | PAW\_PBE Th 07Sep2000     |
| Ti      | PAW\_PBE Ti\_pv 07Sep2000 |
| Tl      | PAW\_PBE Tl\_d 06Sep2000  |
| Tm      | PAW\_PBE Tm\_3 06Sep2000  |
| U       | PAW\_PBE U 06Sep2000      |
| V       | PAW\_PBE V\_pv 07Sep2000  |
| W       | PAW\_PBE W\_pv 06Sep2000  |
| Xe      | PAW\_PBE Xe 07Sep2000     |
| Y       | PAW\_PBE Y\_sv 06Sep2000  |
| Yb      | PAW\_PBE Yb\_2 06Sep2000  |
| Zn      | PAW\_PBE Zn 06Sep2000     |
| Zr      | PAW\_PBE Zr\_sv 07Sep2000 |

### 1st-row elements

$$\text{B, C, N, O, F}$$

Usually, they have three pseudopotentials: a soft \_s, a hard \_h, and a standard. The standard is recommended by VASP and will be used for all. The hard ones have extremely high cut-offs (700 eV)

### alkali and alkali-earth

The table below indicates our choices. Basically, we chose all high e- pseudopotentials except for Na where we excluded `Na_sv` due to its very high cutoff (700 eV).

| element | options            | VASP   | Low elec: oxide form\_enth (exp-comp) eV per fu | High elec: oxide form\_enth (exp-comp) eV per fu | High e- conv. Stats | our choice | rem                                                                                                  |
| ------- | ------------------ | ------ | ----------------------------------------------- | ------------------------------------------------ | ------------------- | ---------- | ---------------------------------------------------------------------------------------------------- |
| Li      | Li, Li\_sv         | Li\_sv | 0.03                                            | 0.01                                             | all converged       | Li\_sv     | highest e- psp chosen                                                                                |
| Na      | Na, Na\_sv, Na\_pv | Na\_pv | 0.06                                            | 0.01                                             | all converged       | Na\_pv     | Na\_sv is extremely high in cutoff (700 eV) for marginal gain in accuracy on Na2O                    |
| K       | K\_pv, K\_sv       | K\_sv  | 0.01                                            | 0.01                                             | 80% conv for both   | K\_sv      | highest e- psp chosen                                                                                |
| Cs      | Cs\_sv             | Cs\_sv |                                                 |                                                  |                     | Cs\_sv     |                                                                                                      |
| Rb      | Rb\_pv, Rb\_sv     | Rb\_sv | 0.05                                            | 0.03                                             | all converged       | Rb\_sv     | highest e- psp chosen                                                                                |
| Be      | Be, Be\_sv         | Be     | 0.04                                            | 0.04                                             | all converged       | Be\_sv     | highest e- psp chosen                                                                                |
| Mg      | Mg, Mg\_pv         | Mg\_pv | 0.02                                            | 0.05                                             | all converged       | Mg\_pv     | VASP and thermo suggest Mg as they are not much different; we decided to stick with the high e- psp. |
| Ca      | Ca\_sv, Ca\_pv     | Ca\_pv | 0.06                                            | 0.03                                             | all converged       | Ca\_sv     | highest e- psp chosen                                                                                |
| Sr      | Sr\_sv             | Sr\_sv |                                                 |                                                  |                     | Sr\_sv     |                                                                                                      |
| Ba      | Ba\_sv             | Ba\_sv |                                                 |                                                  |                     | Ba\_sv     |                                                                                                      |
|         |                    |        |                                                 |                                                  |                     |            |                                                                                                      |

### d-elements, transition metals

The table below shows the details on the PSP choices. All high e- PSPs have been chosen except for Pd which had convergences problem with the high e- PSP in PdO.

<table><thead><tr><th>element</th><th width="229">options</th><th>VASP</th><th>Low elec: oxide form_enth (exp-comp) eV per fu</th><th>High elec: oxide form_enth (exp-comp) eV per fu</th><th>High e- conv. Stats</th><th>our choice</th><th>rem</th></tr></thead><tbody><tr><td>Sc</td><td>Sc_sv</td><td>Sc_sv</td><td></td><td></td><td></td><td>Sc_sv</td><td></td></tr><tr><td>Y</td><td>Y_sv</td><td>Y_sv</td><td></td><td></td><td></td><td>Y_sv</td><td></td></tr><tr><td>Ti</td><td>Ti, Ti_pv, Ti_sv</td><td>Ti_pv</td><td>0.13</td><td>0.23</td><td>metal conv pb with Ti and Ti_sv</td><td>Ti_pv</td><td>highest e- psp with best conv. chosen</td></tr><tr><td>Zr</td><td>Zr, Zr_sv</td><td>Zr_sv</td><td>0.06</td><td>0.03</td><td>all converged</td><td>Zr_sv</td><td>highest e- psp chosen</td></tr><tr><td>Hf</td><td>Hf, Hf_pv</td><td>Hf_pv</td><td>0.19</td><td>0.18</td><td>all converged</td><td>Hf_pv</td><td>highest e- psp chosen</td></tr><tr><td>V</td><td>V, V_pv, V_sv</td><td>V_pv</td><td>0.39</td><td>0.46</td><td>all converged</td><td>V_pv</td><td>balance of high e-psp and compute cost</td></tr><tr><td>Nb</td><td>Nb_pv</td><td>Nb_pv</td><td></td><td></td><td></td><td>Nb_pv</td><td></td></tr><tr><td>Ta</td><td>Ta, Ta_pv</td><td>Ta_pv</td><td>0.3</td><td>0.31</td><td>similar conv. for both</td><td>Ta_pv</td><td>highest e- psp chosen</td></tr><tr><td>Cr</td><td>Cr, Cr_pv</td><td>Cr_pv</td><td>0.53</td><td>0.6</td><td>all converged</td><td>Cr_pv</td><td>highest e- psp chosen</td></tr><tr><td>Mo</td><td>Mo, Mo_pv</td><td>Mo_pv</td><td>0.39</td><td>0.45</td><td>all converged</td><td>Mo_pv</td><td>highest e- psp chosen</td></tr><tr><td>W</td><td>W, W_pv</td><td>W_pv</td><td>0.47</td><td>0.48</td><td>all converged</td><td>W_pv</td><td>highest e- psp chosen</td></tr><tr><td>Mn</td><td>Mn, Mn_pv</td><td>Mn or Mn_pv (!)</td><td>0.29</td><td>0.31</td><td>all converged</td><td>Mn_pv</td><td>highest e- psp chosen</td></tr><tr><td>Tc</td><td>Tc, Tc_pv</td><td>Tc or Tc_pv</td><td></td><td></td><td>all converged (no metals BTW)</td><td>Tc_pv</td><td>highest e- psp chosen</td></tr><tr><td>Re</td><td>Re, Re_pv</td><td>Re</td><td>0.56</td><td>0.59</td><td>all converged</td><td>Re_pv</td><td>highest e- psp chosen</td></tr><tr><td>Fe</td><td>Fe, Fe_pv</td><td>Fe_pv</td><td>0.62</td><td>0.47</td><td>50% conv. on oxides for both psp</td><td>Fe_pv</td><td>highest e- psp chosen</td></tr><tr><td>Co</td><td>Co</td><td>Co</td><td></td><td></td><td></td><td>Co</td><td></td></tr><tr><td>Ni</td><td>Ni, Ni_pv</td><td>Ni</td><td>0.4</td><td>0.4</td><td>all converged</td><td>Ni_pv</td><td>highest e- psp chosen</td></tr><tr><td>Cu</td><td>Cu, Cu_pv</td><td>Cu</td><td>0.07</td><td>0.1</td><td>all converged</td><td>Cu_pv</td><td>highest e- psp chosen</td></tr><tr><td>Zn</td><td>Zn</td><td>Zn</td><td></td><td></td><td></td><td>Zn</td><td></td></tr><tr><td>Ru</td><td>Ru, Ru_pv</td><td>Ru</td><td>0.41</td><td>0.41</td><td>all converged</td><td>Ru_pv</td><td>highest e- psp chosen</td></tr><tr><td>Rh</td><td>Rh, Rh_pv</td><td>Rh</td><td>0.36</td><td>0.35</td><td>all converged</td><td>Rh_pv</td><td>highest e- psp chosen</td></tr><tr><td>Pd</td><td>Pd, Pd_pv</td><td>Pd</td><td>0.2</td><td>0.2</td><td>Pd_pv has one unconv. PdO</td><td>Pd</td><td>due to the conv. issue we chose Pd (recommended by VASP too).</td></tr><tr><td>Ag</td><td>Ag</td><td></td><td></td><td></td><td></td><td>Ag</td><td></td></tr><tr><td>Cd</td><td>Cd</td><td></td><td></td><td></td><td></td><td>Cd</td><td></td></tr><tr><td>Hg</td><td>Hg</td><td></td><td></td><td></td><td></td><td>Hg</td><td></td></tr><tr><td>Au</td><td>Au</td><td></td><td></td><td></td><td></td><td>Au</td><td></td></tr><tr><td>Ir</td><td>Ir</td><td></td><td></td><td></td><td></td><td>Ir</td><td></td></tr><tr><td>Pt</td><td>Pt</td><td>Pt</td><td></td><td></td><td></td><td>Pt</td><td></td></tr><tr><td>Os</td><td>Os, Os_pv</td><td>Os_pv</td><td>0.67</td><td>0.7</td><td>all converged</td><td>Os_pv</td><td>highest e- psp chosen</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr></tbody></table>

### main group

Si, P, Cl, S will be used in their standard form (not hard) as suggested by VASP manual.

The `Al_h` psp was found to be definitely wrong in terms of band structure. There were "ghost" states found in the DOS.

Pb is interesting as the high e- psp shows significantly higher error in formation energies. We kept the high e- psp (`Pb_d`), but it might be interesting to study this a little more. One hypothesis relies on a recent result showing that lead oxide formation energies need the use of spin-orbit coupling to be accurate. [\[2\]](pseudopotentials.md#references) Our computations do not include any relativistic corrections for valence electrons. However, spin-orbit coupling is taken into account during the psp construction. This would explain why a psp with more core electrons (treated indirectly with spin-orbit coupling) would give more accurate results than a psp with fewer electrons.

`Bi_d` shows a convergence problem, so the decision on Bi has been postponed to further analysis.

Finally, Po and At, while referred to in the VASP manual, are not present in the VASP PAW library.

| element | options          | VASP  | Low elec: oxide form\_enth (exp-comp) eV per fu | High elec: oxide form\_enth (exp-comp) eV per fu | High e- conv. Stats | our choice | rem                                                                           |
| ------- | ---------------- | ----- | ----------------------------------------------- | ------------------------------------------------ | ------------------- | ---------- | ----------------------------------------------------------------------------- |
| Ga      | Ga, Ga\_d, Ga\_h | Ga\_d | 0.05                                            | 0.01                                             | all converged       | Ga\_d      | Ga\_h seems best (0.01 instead of 0.02) but same problem as Al\_h?            |
| Ge      | Ge, Ge\_d, Ge\_h | Ge\_d | 0.06                                            | 0.06                                             | all converged       | Ge\_d      | Ge\_h seems best (Ge\_h and Ge\_d similar though) but same problem as Al\_h ? |
| Al      | Al, Al\_h        | Al    | 0.03                                            | 0.01                                             | all converged       | Al         | Good energetics but pb in band structure                                      |
| As      |                  |       |                                                 |                                                  |                     | As         |                                                                               |
| Se      |                  |       |                                                 |                                                  |                     | Se         |                                                                               |
| Br      |                  |       |                                                 |                                                  |                     | Br         |                                                                               |
| In      | In, In\_d        | In\_d | 0.13                                            | 0.1                                              | all converged       | In\_d      | highest e- psp chosen                                                         |
| Sn      | S, Sn\_d         | Sn\_d | 0.16                                            | 0.12                                             | all converged       | Sn\_d      | highest e- psp chosen                                                         |
| Tl      | Tl, Tl\_d        | Tl\_d | 0.26                                            | 0.31                                             | all converged       | Tl\_d      | highest e- psp chosen                                                         |
| Pb      | Pb, Pb\_d        | Pb\_d | 0.17                                            | 0.36                                             | all converged       | Pb\_d      | highest e- psp chosen                                                         |
| Bi      | Bi, Bi\_d        | Bi\_d |                                                 |                                                  | convergence pb      | ?          |                                                                               |
| Po      | Po, Po\_d        |       |                                                 |                                                  |                     | Po         | no Po psp is available in the PAW library!                                    |
| At      | At, At\_d        |       |                                                 |                                                  |                     | At\_d      | no At psp is available in the PAW library                                     |

### rare-earth, f-electrons

These are probably the most problematic to use as pseudopotentials. Here is what the VASP manual says about them:

> Due to self-interaction errors, f-electrons are not handled well by presently available density functionals. In particular, partially filled states are often incorrectly described, leading to large errors for Pr-Eu and Tb-Yb where the error increases in the middle (Gd is handled reasonably well, since 7 electrons occupy the majority shell). These errors are DFT and not VASP related. Particularly problematic is the description of the transition from an itinerant (band-like) behavior observed at the beginning of each period to localized states towards the end of the period. For the elements, this transition occurs already in La and Ce, whereas the transition sets in for Pu and Am for the elements. A routine way to cope with the inabilities of present DFT functionals to describe the localized electrons is to place the electrons in the core. Such potentials are available and described below. Furthermore, PAW potentials in which the states are treated as valence states are available, but these potentials are not expected to work reliable when the electrons are localized.

In summary, the pseudopotentials can either include or not include f electrons; how accurate including them or not is depends on the nature of the bonding for each particular system (localized or not).

What we found is that convergence issues are often seen for high electron psp (e.g., Pr, Nd, Sm). Also, some pseudopotentials (e.g., `Er_2`, `Eu_2`) freeze too many electrons and therefore have issues with oxidation states that make one of the frozen electron participate in bonding (e.g., Eu2O3, Er2O3). Finally, there is a major problem with Tb. Only `Tb_3` exists but Tb is known to also form Tb4+ compounds (e.g., TbO2). For those Tb4+ compounds, this psp is likely to be extremely wrong. There is currently no fix for this except waiting for someone to develop a PAW `Tb_4` psp.

| element | options          | VASP | Low elec: oxide form\_enth (exp-comp) eV per fu | High elec: oxide form\_enth (exp-comp) eV per fu | High e- conv. Stats                  | our choice | rem                                                                                                              |
| ------- | ---------------- | ---- | ----------------------------------------------- | ------------------------------------------------ | ------------------------------------ | ---------- | ---------------------------------------------------------------------------------------------------------------- |
| La      | La, La\_s        | La   | 0.12                                            | 0.17                                             | all converged                        | La         | La\_s means soft                                                                                                 |
| Ce      | Ce\_3, Ce        | /    | 1.18                                            | 0.26                                             | all converged                        | Ce         | thermo data on CeO2 is terrible with Ce\_3, cf Ce4+ thermo data on Ce2O3 is similar with both                    |
| Pr      | Pr\_3, Pr        | /    | 0.00                                            | 0.09                                             | Pr metal did not converge            | Pr\_3      | Pr\_3 better oxide thermo (surprisingly good!) and convergence in metal.                                         |
| Nd      | Nd\_3, Nd        | /    | 0.04                                            | 0.01                                             | Nd metal conv. problem               | Nd\_3      | convergence pb                                                                                                   |
| Pm      | Pm\_3, Pm        | /    | /                                               | /                                                |                                      | Pm\_3      | no real data to compare, it is between Nd and Sm in the periodic table, so we decided to pick a \_3 as Nd and Sm |
| Sm      | Sm\_3, Sm        | /    | 0.1                                             | /                                                | Sm metal conv. pb                    | Sm\_3      | conv pb                                                                                                          |
| Eu      | Eu\_2, Eu        | /    | 0.68                                            | 0.25                                             | all converged                        | Eu         | Both EuO and Eu2O3 thermo worse with Eu\_2                                                                       |
| Gd      | Gd\_3, Gd        | /    | 0.2                                             | 0.12                                             | all converged                        | Gd         | Gd has better thermo and highest e-                                                                              |
| Tb      | Tb\_3            | /    |                                                 |                                                  | all converged                        | Tb\_3      | There is a major pb with Tb. It can 4+ and we have only a 3+ psps                                                |
| Dy      | Dy\_3            | /    |                                                 |                                                  | all converged                        | Dy\_3      |                                                                                                                  |
| Ho      | Ho\_3            | /    |                                                 |                                                  |                                      | Ho\_3      |                                                                                                                  |
| Er      | Er\_2, Er\_3     | /    | 1.16                                            | 0.15                                             | all converged                        | Er\_3      | thermo data on Er2O3 off with Er\_2                                                                              |
| Tm      | Tm, Tm\_3        | /    | 0.2                                             | ?                                                | could not converge any metal with Tm | Tm\_3      |                                                                                                                  |
| Yb      | Yb\_3, Yb\_2, Yb | /    | 1.03                                            | 0.59                                             | all converged                        | Yb\_3      | thermo data off with Yb\_2 and Yb has convergence issues                                                         |
| Lu      | Lu\_3, Lu        | /    | 0.43                                            | ?                                                | Lu could not be converged            | Lu\_3      |                                                                                                                  |

### transuranides, f-electrons

U, Ac, Th, Pa, Np, Pu, Am

Following VASP suggestion, we decided to use the standard (and not the soft) version for all those pseudopotentials.

## Citation

To cite the Materials Project, please reference the following work:

M. K. Horton, P. Huck, R. X. Yang, J. M. Munro, S. Dwaraknath, A. M. Ganose, R. S. Kingsbury, M. Wen, J. X.\
Shen, T. S. Mathis, A. D. Kaplan, K. Berket, J. Riebesell, J. George, A. S. Rosen, E. W. C. Spotte-Smith, M. J.\
McDermott, O. A. Cohen, A. Dunn, M. C. Kuner, G.-M. Rignanese, G. Petretto, D. Waroquiers, S. M. Griffin,\
J. B. Neaton, D. C. Chrzan, M. Asta, G. Hautier, S. Cholia, G. Ceder, S. P. Ong, A. Jain, and K. A. Persson,\
Nature Materials, yr. 2025, DOI: [10.1038/s41563-025-02272-0](https://doi.org/10.1038/s41563-025-02272-0)

#### Past citations

To cite the legacy data in the Materials Project (from [https://legacy.materialsproject.org/](https://legacy.materialsproject.org/)), please cite:

A. Jain, G. Hautier, C. J. Moore, S. P. Ong, C. C. Fischer, T. Mueller, K. A. Persson, and G. Ceder, A high-throughput infrastructure for density functional theory calculations, Computational Materials Science, vol. 50, yr. 2011, pp. 2295-2310. [DOI:10.1016/j.commatsci.2011.02.023](https://dx.doi.org/10.1016/j.commatsci.2011.02.023)

## Authors

1. Geoffroy Hautier
2. Aaron Kaplan
3. Michael Wolloch (VASP)

## References

\[1]: P.E. Blöchl, Physical Review B 50, 17953-17979 (1994).&#x20;

\[2]: R. Ahuja, A. Blomqvist, P. Larsson, P. Pyykkö, and P. Zaleski-Ejgierd, Physical Review Letters 106, 1-4 (2011).
