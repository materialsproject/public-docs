# Pseudopotentials

Pseudopotentials are used to reduce computation time by replacing the full electron system in the coulombic potential by a system only taking explicitly into account the "valence" electrons (i.e., the electrons participating into bonding) but in a pseudopotential. This approach not only reduces the electron number but also the energy cutoff necessary (this is critical in plane-wave based computations). All computations in the materials project have been performed using a specific type of very efficient pseudopotentials: the projector augmented wave (PAW) pseudopotentials. [\[1\]](pseudopotentials.md#references) We used the library of PAW pseudopotentials provided by VASP but for a given element there are often several possibilities in the VASP library. This wiki presents how the choices between the different pseudopotential options were made.

## The strategy

As a test set, we ran all elements and binary oxides present in the ICSD with the available PAW pseudopotentials. As it is difficult to test for all properties (structural, electronic, etc...), we chose to be inclusive and to select the pseudopotential (psp) with the largest number of electrons (high e) EXCEPT if convergence issues were seen on our test set, or if previous experience excluded a specific pseudopotential. We also excluded pseudopotentials with too large an energy cutoff.

We also compared to recommendations from the VASP manual present in [1](https://www.vasp.at/wiki/index.php/Available\_PAW\_potentials).

Finally, as we had energies for elements and binary oxides, we compared binary oxide formation energies with the available pseudopotentials. The oxygen molecule energy was obtained from Wang et al. Please note that this data is pure GGA and some chemistries (e.g., transition metals) will give extremely bad formation energy results in GGA. This is not an issue with the pseudopotential but with the functional, so we do not focus on that issue in this wiki.

## Pseudopotential comments and choice

### 1st row elements

$$\text{B, C, N, O, F}$$

Usually they have three pseudopotentials: a soft \_s, a hard \_h, and a standard. The standard is recommended by VASP and will be used for all. The hard have extremely high cut-offs (700 eV)

### alkali and alkali-earth

The table below indicates our choices. Basically, we chose all high e- pseudopotentials except for Na where we excluded Na\_sv due to its very high cutoff (700 eV).

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

The table below shows the details on the psp choices. All high e- psp have been chosen except for Pd which had convergences problem with the high e- psp in PdO.

| element | options            | VASP             | Low elec: oxide form\_enth (exp-comp) eV per fu | High elec: oxide form\_enth (exp-comp) eV per fu | High e- conv. Stats              | our choice | rem                                                           |
| ------- | ------------------ | ---------------- | ----------------------------------------------- | ------------------------------------------------ | -------------------------------- | ---------- | ------------------------------------------------------------- |
| Sc      | Sc\_sv             | Sc\_sv           |                                                 |                                                  |                                  | Sc\_sv     |                                                               |
| Y       | Y\_sv              | Y\_sv            |                                                 |                                                  |                                  | Y\_sv      |                                                               |
| Ti      | Ti, Ti\_pv, Ti\_sv | Ti\_pv           | 0.13                                            | 0.23                                             | metal conv pb with Ti and Ti\_sv | Ti\_pv     | highest e- psp with best conv. chosen                         |
| Zr      | Zr, Zr\_sv         | Zr\_sv           | 0.06                                            | 0.03                                             | all converged                    | Zr\_sv     | highest e- psp chosen                                         |
| Hf      | Hf, Hf\_pv         | Hf\_pv           | 0.19                                            | 0.18                                             | all converged                    | Hf\_pv     | highest e- psp chosen                                         |
| V       | V, V\_pv, V\_sv    | V\_pv            | 0.39                                            | 0.46                                             | all converged                    | V\_sv      | highest e- psp chosen                                         |
| Nb      | Nb\_pv             | Nb\_pv           |                                                 |                                                  |                                  | Nb\_pv     |                                                               |
| Ta      | Ta, Ta\_pv         | Ta\_pv           | 0.3                                             | 0.31                                             | similar conv. for both           | Ta\_pv     | highest e- psp chosen                                         |
| Cr      | Cr, Cr\_pv         | Cr\_pv           | 0.53                                            | 0.6                                              | all converged                    | Cr\_pv     | highest e- psp chosen                                         |
| Mo      | Mo, Mo\_pv         | Mo\_pv           | 0.39                                            | 0.45                                             | all converged                    | Mo\_pv     | highest e- psp chosen                                         |
| W       | W, W\_pv           | W\_pv            | 0.47                                            | 0.48                                             | all converged                    | W\_pv      | highest e- psp chosen                                         |
| Mn      | Mn, Mn\_pv         | Mn or Mn\_pv (!) | 0.29                                            | 0.31                                             | all converged                    | Mn\_pv     | highest e- psp chosen                                         |
| Tc      | Tc, Tc\_pv         | Tc or Tc\_pv     |                                                 |                                                  | all converged (no metals BTW)    | Tc\_pv     | highest e- psp chosen                                         |
| Re      | Re, Re\_pv         | Re               | 0.56                                            | 0.59                                             | all converged                    | Re\_pv     | highest e- psp chosen                                         |
| Fe      | Fe, Fe\_pv         | Fe\_pv           | 0.62                                            | 0.47                                             | 50% conv. on oxides for both psp | Fe\_pv     | highest e- psp chosen                                         |
| Co      | Co                 | Co               |                                                 |                                                  |                                  | Co         |                                                               |
| Ni      | Ni, Ni\_pv         | Ni               | 0.4                                             | 0.4                                              | all converged                    | Ni\_pv     | highest e- psp chosen                                         |
| Cu      | Cu, Cu\_pv         | Cu               | 0.07                                            | 0.1                                              | all converged                    | Cu\_pv     | highest e- psp chosen                                         |
| Zn      | Zn                 | Zn               |                                                 |                                                  |                                  | Zn         |                                                               |
| Ru      | Ru, Ru\_pv         | Ru               | 0.41                                            | 0.41                                             | all converged                    | Ru\_pv     | highest e- psp chosen                                         |
| Rh      | Rh, Rh\_pv         | Rh               | 0.36                                            | 0.35                                             | all converged                    | Rh\_pv     | highest e- psp chosen                                         |
| Pd      | Pd, Pd\_pv         | Pd               | 0.2                                             | 0.2                                              | Pd\_pv has one unconv. PdO       | Pd         | due to the conv. issue we chose Pd (recommended by VASP too). |
| Ag      | Ag                 |                  |                                                 |                                                  |                                  | Ag         |                                                               |
| Cd      | Cd                 |                  |                                                 |                                                  |                                  | Cd         |                                                               |
| Hg      | Hg                 |                  |                                                 |                                                  |                                  | Hg         |                                                               |
| Au      | Au                 |                  |                                                 |                                                  |                                  | Au         |                                                               |
| Ir      | Ir                 |                  |                                                 |                                                  |                                  | Ir         |                                                               |
| Pt      | Pt                 | Pt               |                                                 |                                                  |                                  | Pt         |                                                               |
| Os      | Os, Os\_pv         | Os\_pv           | 0.67                                            | 0.7                                              | all converged                    | Os\_pv     | highest e- psp chosen                                         |
|         |                    |                  |                                                 |                                                  |                                  |            |                                                               |

### main group

Si, P, Cl, S will be used in their standard form (not hard) as suggested by VASP manual.

The Al\_h psp was found to be definitely wrong in terms of band structure. There were "ghost" states found in the DOS.

Pb is interesting as the high e- psp shows significantly higher error in formation energies. We kept the high e- psp (Pb\_d), but it might be interesting to study this a little more. One hypothesis relies on a recent result showing that lead oxide formation energies need the use of spin-orbit coupling to be accurate. [\[2\]](pseudopotentials.md#references) Our computations do not include any relativistic corrections for valence electrons. However, spin-orbit coupling is taken into account during the psp construction. This would explain why a psp with more core electrons (treated indirectly with SO coupling) would give more accurate results than a psp with fewer electrons.

Bi\_d shows a convergence problem, so the decision on Bi has been postponed to further analysis.

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

"Due to self-interaction errors, f-electrons are not handled well by presently available density functionals. In particular, partially filled states are often incorrectly described, leading to large errors for Pr-Eu and Tb-Yb where the error increases in the middle (Gd is handled reasonably well, since 7 electrons occupy the majority shell). These errors are DFT and not VASP related. Particularly problematic is the description of the transition from an itinerant (band-like) behavior observed at the beginning of each period to localized states towards the end of the period. For the elements, this transition occurs already in La and Ce, whereas the transition sets in for Pu and Am for the elements. A routine way to cope with the inabilities of present DFT functionals to describe the localized electrons is to place the electrons in the core. Such potentials are available and described below. Furthermore, PAW potentials in which the states are treated as valence states are available, but these potentials are not expected to work reliable when the electrons are localized."

In summary, the pseudopotentials can either include or not include f electrons; how accurate including them or not is depends on the nature of the bonding for each particular system (localized or not).

What we found is that convergence issues are often seen for high electron psp (e.g., Pr, Nd, Sm). Also, some pseudopotentials (e.g., Er\_2, Eu\_2) freeze too many electrons and therefore have issues with oxidation states that make one of the frozen electron participate in bonding (e.g., Eu2O3, Er2O3). Finally, there is a major problem with Tb. Only Tb\_3 exists but Tb is known to also form Tb4+ compounds (e.g., TbO2). For those Tb4+ compounds, this psp is likely to be extremely wrong. There is not much fix to this for now except waiting for someone to develop a PAW Tb\_4 psp.

| element | options      | VASP | Low elec: oxide form\_enth (exp-comp) eV per fu | High elec: oxide form\_enth (exp-comp) eV per fu | High e- conv. Stats                  | our choice | rem                                                                                                              |
| ------- | ------------ | ---- | ----------------------------------------------- | ------------------------------------------------ | ------------------------------------ | ---------- | ---------------------------------------------------------------------------------------------------------------- |
| La      | La, La\_s    | La   | 0.12                                            | 0.17                                             | all converged                        | La         | La\_s means soft                                                                                                 |
| Ce      | Ce\_3, Ce    | /    | 1.18                                            | 0.26                                             | all converged                        | Ce         | thermo data on CeO2 is terrible with Ce\_3, cf Ce4+ thermo data on Ce2O3 is similar with both                    |
| Pr      | Pr\_3, Pr    | /    | 0.00                                            | 0.09                                             | Pr metal did not converge            | Pr\_3      | Pr\_3 better oxide thermo (surprisingly good!) and convergence in metal.                                         |
| Nd      | Nd\_3, Nd    | /    | 0.04                                            | 0.01                                             | Nd metal conv. problem               | Nd\_3      | convergence pb                                                                                                   |
| Pm      | Pm\_3, Pm    | /    | /                                               | /                                                |                                      | Pm\_3      | no real data to compare, it is between Nd and Sm in the periodic table, so we decided to pick a \_3 as Nd and Sm |
| Sm      | Sm\_3, Sm    | /    | 0.1                                             | /                                                | Sm metal conv. pb                    | Sm\_3      | conv pb                                                                                                          |
| Eu      | Eu\_2, Eu    | /    | 0.68                                            | 0.25                                             | all converged                        | Eu         | Both EuO and Eu2O3 thermo worse with Eu\_2                                                                       |
| Gd      | Gd\_3, Gd    | /    | 0.2                                             | 0.12                                             | all converged                        | Gd         | Gd has better thermo and highest e-                                                                              |
| Tb      | Tb\_3        | /    |                                                 |                                                  | all converged                        | Tb\_3      | There is a major pb with Tb. It can 4+ and we have only a 3+ psps                                                |
| Dy      | Dy\_3        | /    |                                                 |                                                  | all converged                        | Dy\_3      |                                                                                                                  |
| Ho      | Ho\_3        | /    |                                                 |                                                  |                                      | Ho\_3      |                                                                                                                  |
| Er      | Er\_2, Er\_3 | /    | 1.16                                            | 0.15                                             | all converged                        | Er\_3      | thermo data on Er2O3 off with Er\_2                                                                              |
| Tm      | Tm, Tm\_3    | /    | 0.2                                             | ?                                                | could not converge any metal with Tm | Tm\_3      |                                                                                                                  |
| Yb      | Yb\_2, Yb    | /    | 1.03                                            | 0.59                                             | all converged                        | Yb         | thermo data off with Yb\_2                                                                                       |
| Lu      | Lu\_3, Lu    | /    | 0.43                                            | ?                                                | Lu could not be converged            | Lu\_3      |                                                                                                                  |

### transuranides, f-electrons

U, Ac, Th, Pa, Np, Pu, Am

Following vasp suggestion, we decided to use the standard (and not the soft) version for all those pseudopotentials.

## Citation

To cite the Materials Project, please reference the following work:

A. Jain, G. Hautier, C. J. Moore, S. P. Ong, C. C. Fischer, T. Mueller, K. A. Persson, and G. Ceder, A high-throughput infrastructure for density functional theory calculations, Computational Materials Science, vol. 50, 2011, pp. 2295-2310. [DOI:10.1016/j.commatsci.2011.02.023](https://dx.doi.org/10.1016/j.commatsci.2011.02.023)

## Authors

1. Geoffroy Hautier

## References

\[1]: P.E. Blöchl, Physical Review B 50, 17953-17979 (1994). 

\[2]: R. Ahuja, A. Blomqvist, P. Larsson, P. Pyykkö, and P. Zaleski-Ejgierd, Physical Review Letters 106, 1-4 (2011).
