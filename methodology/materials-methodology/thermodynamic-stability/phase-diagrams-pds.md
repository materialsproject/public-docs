---
description: >-
  A description of the methodology for constructing and interpreting
  compositional phase diagrams as used on the Materials Project (MP) website.
  These are available as part of the Phase Diagram app.
---

# Phase Diagrams (PDs)

## Introduction

A **phase diagram** is a calculation of the _thermodynamic phase equilibria_ of multicomponent systems. It is an important tool in materials science for revealing 1) thermodynamic stability of compounds, 2) predicted equilibrium chemical reactions, and 3) processing conditions for synthesizing materials. However, the experimental determination of a phase diagram is an extremely time-consuming process, requiring careful synthesis and characterization of all phases in a chemical system.

Computational modeling tools, such as the density functional theory (DFT) methods used by the Materials Project, can accelerate compositional phase diagram construction significantly. By calculating the energies of all known compounds in a given chemical system (e.g. the lithium/iron/oxygen chemical system, Li-Fe-O), we can determine the phase diagram for that system at a temperature of $$T=0$$ K and pressure of $$P=0$$ **** atm. Furthermore, for systems comprised of predominantly solid phases open with respect to a gaseous element, approximations can be made as to the finite temperature and pressure phase diagrams.

In this section, we will describe the theory/methodology behind the calculation of compositional phase diagrams.

## Methodology

This section will discuss how to construct phase diagrams from DFT-calculated energies. This is exact process done by the Materials Project (MP) for computing formation energies, thermodynamic stability, and phase diagrams. This methodology has been implemented in Python within the **pymatgen** package. Please see [#code-pymatgen](phase-diagrams-pds.md#code-pymatgen "mention")for brief examples of how to build phase diagrams on your own.

### Acquiring thermodynamic data



### Calculating formation energy

The formation energy, $$\Delta E_f$$, is the energy change upon reacting to form a phase of interest from its constituent components. The components typically used are the constituent elements. For a phase composed of $$N$$ components indexed by $$i$$, the formation energy can be calculated as follows:

$$\Delta E_f  = E - \sum_i^N{n_i\mu_i}$$

where $$E$$ is the total energy of the phase of interest, $$n_i$$is the total number of moles of component $$i$$, and $$\mu_i$$ is the total energy of component $$i$$. **Note** that $$\mu_i$$is often referred to as the chemical potential of the component, however, this is only rigorously true when working with Gibbs free energies, $$G$$.

{% hint style="info" %}
**Example:**

For barium titanate, BaTiO$$_3$$, the formation energy would be calculated as:

$$\Delta E_f (BaTiO_3) = E(BaTiO_3) - 1*\mu_{Ba} - 1*\mu_{Ti} - 3*\mu_{O}$$&#x20;
{% endhint %}

Typically, formation energies are **normalized** on a per-atom basis by dividing by the number of atoms in 1 mole of formula. For example, for BaTiO$$_3$$, the normalized per-atom formation energy would be calculated by dividing the $$E_f$$ above by _5 atoms._

### Constructing the compositional phase diagram

#### The convex hull approach



$$
\eqalign{G(T,P,N_{Li},N_{Fe},N_{O}) &= H(T,P,N_{Li},N_{Fe},N_{O}) - TS(T,P,N_{Li},N_{Fe},N_{O})\cr &= \eqalign{E(T,P,N_{Li},N_{Fe},N_{O}) + PV(T,P,N_{Li},N_{Fe},N_{O})\cr - TS(T,P,N_{Li},N_{Fe},N_{O}}}
$$

#### Evaluating thermodyanmic stability

![Figure 2: Illustration of various thermodynamic stability metrics, reproduced from Bartel \[1\].#](../../../.gitbook/assets/10853\_2022\_6915\_Fig1\_HTML.webp)

#### Visualizing the phase diagram



### Grand potential phase diagram

In many scientific applications, the phase equilibria of interest is not that of a closed system, but rather one which is open to a particular element. For instance, synthesis might be carried out in flowing Argon gas at a particular temperature, which would mean the system is open to gaseous elements such as oxygen. Other experiments may be carried out in air, which essentially serves as an infinite reservoir of atmospheric elements such as oxygen, nitrogen and others.

In environments that are open to a particular element (say oxygen), the relevant control variable is the chemical potential of that element, $\mu\_{O}$. The relevant thermodynamic potential to study phase equilibria in an open system is the grand potential, which is defined as the following for an $\ce{Li-Fe-O}$ system that is open with respect to oxygen:

$$
\eqalign{\phi(T,P,N_{Li},N_{Fe},\mu_{O}) &= G(T,P,N_{Li},N_{Fe},\mu_{O}) - \mu_ON_O(T,P,N_{Li},N_{Fe},\mu_{O})\cr &= \eqalign{E(T,P,N_{Li},N_{Fe},\mu_{O}) - TS(T,P,N_{Li},N_{Fe},N_{O})\cr - \mu_ON_O(T,P,N_{Li},N_{Fe},\mu_{O})}}
$$

where the $PV$ term is again ignored.

Normalizing with respect to $\ce{Li-Fe}$ composition and dropping the explicit expression of the functional dependence of $E$, $S$, and $N\_{O}$ on the right-hand side henceforth for brevity, we can obtain

$$\bar{\phi}(T,P,x_{Li},x_{Fe},\mu_{O}) = \frac{E-TS-\mu_ON_O}{N_{Li} + N_{Fe}}$$

where $x\_i = \frac{N\_i}{N\_{Li}+N\_{Fe\}}$ is the fraction of component $i$ in $\ce{Li-Fe}$ composition space.

To formally introduce temperature into ab initio phase stability calculations, one would usually need to take into account all the relevant excitations (e.g. vibrational, configurational, and electronic) that contribute to entropy. However, for systems where phase equilibria changes takes place primarily between solid phases with absorption or loss of as gas, a few simplifying assumptions can be made that allow us to obtain a useful approximate phase diagram with less effort. In such reactions, the reaction entropy is dominated by the entropy of the gas, and the effect of temperature is mostly captured by changes in the chemical potential of the gas. Since the $TS$ term is the entropy contribution of the condensed system, it can be neglected compared to the entropy effect of the gas, and the expression for $\bar{\phi}$ simplifies to:

$$\bar{\phi} = \frac{E-\mu_ON_O}{N_{Li} + N_{Fe}}$$

Using the above assumption, the effect of temperature and partial pressure can be fully captured in a single chemical potential variable, with a more negative value corresponding to higher temperatures or lower partial pressures. The chemical potential is then treated as an external variable to obtain a grand potential phase diagram under a particular condition by taking the convex hull of $\bar{\phi}$ for all phases and projecting the stable nodes into the $\ce{Li-Fe}$ composition space.

## **Accuracy of Calculated Phase Diagrams**

A compositional phase diagram is&#x20;

!\[Experimental Li-Fe-O Phase Diagram]\(/img/phase-diagram/Li-Fe-O\_exp.png "Experimental Li-Fe-O Phase Diagram")

_Figure 4: Experimental Li-Fe-O Phase Diagram_

_Figure 4_ shows the experimentally determined phase diagram for the $\ce{Li-Fe-O}$ system at 673K.\[^5] Even though the experimental phase diagram is at a much higher temperature, we can see that our calculated phase diagram reproduces the features very well. The phases $\ce{Li2O}$, $\ce{Fe3O4}$, $\ce{Fe2O3}$, $\ce{LiFeO2}$, $\ce{Li5FeO4}$ are stable in both the experimental diagram and our calculated phase diagram. The only additional phase in our calculated diagram is $\ce{FeO}$, which is well known to be difficult to obtain in stoichiometric proportion under normal conditions. Our calculations correctly reproduce the $\ce{FeO}$ formation enthalpy to within experimental accuracy, and our calculated phase diagram is fully consistent with known experimental thermochemical data along the $\ce{Fe-O}$ line at 1 atm and 298K.

In general, we can expect that compositional phase diagrams comprising of predominantly solid phases to be reproduced fairly well by our calculations. However, it should be noted that there are inherent limitations in accuracy in the DFT calculated energies. Furthermore, our calculated phase diagrams are at 0 K and 0 atm, and differences with non-zero temperature phase diagrams are to be expected.

For grand potential phase diagrams, further approximations are made as to the entropic contributions.[^2](https://doi.org/10.1016/j.elecom.2010.01.010) They are therefore expected to be less accurate, but nonetheless provide useful insights on general trends.

## Citations

{% hint style="info" %}
### Methodology (I)

[S. P. Ong, L. Wang, B. Kang, G. Ceder., The Li-Fe-P-O2 Phase Diagram from First Principles Calculations, Chemistry of Materials, vol. 20, Mar. 2008, pp. 1798-1807.](https://doi.org/10.1021/cm702327g)
{% endhint %}

{% hint style="info" %}
### Methodology (II)

[S.P. Ong, A. Jain, G. Hautier, B. Kang, and G. Ceder, Thermal stabilities of delithiated olivine MPO4 (M=Fe, Mn) cathodes investigated using first principles calculations, Electrochemistry Communications, vol. 12, 2010, pp. 427-430](https://doi.org/10.1016/j.elecom.2010.01.010).
{% endhint %}

## References

\[1] Bartel, C.J. Review of computational approaches to predict the thermodynamic stability of inorganic solids. _J Mater Sci_ **57,** 10475–10498 (2022).[ https://doi.org/10.1007/s10853-022-06915-4](https://doi.org/10.1007/s10853-022-06915-4)

\[2] V. Raghavan, Fe-Li-O Phase Diagram, ASM Alloy Phase Diagrams Center, P. Villars, editor-in-chief; H. Okamoto and K. Cenzual, section editors; [http://www1.asminternational.org/AsmEnterprise/APD](http://www1.asminternational.org/AsmEnterprise/APD), ASM International, Materials Park, OH, 2006.&#x20;

\[3]: [https://dx.doi.org/10.1145/235815.235821](https://dx.doi.org/10.1145/235815.235821)

## Code (pymatgen)

TODO

