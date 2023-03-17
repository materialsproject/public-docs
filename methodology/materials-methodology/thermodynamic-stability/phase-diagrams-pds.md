---
description: >-
  A description of the methodology for constructing and interpreting
  compositional phase diagrams from the Materials Project (MP) website and API.
---

# Phase Diagrams (PDs)

## Introduction

A **phase diagram** is a calculation of the _thermodynamic phase equilibria_ of multicomponent systems. It is an important tool in materials science for revealing 1) thermodynamic stability of compounds, 2) predicted equilibrium chemical reactions, and 3) processing conditions for synthesizing materials. However, the experimental determination of a phase diagram is an extremely time-consuming process, requiring careful synthesis and characterization of all phases in a chemical system.

Computational modeling tools, such as the density functional theory (DFT) methods used by the Materials Project, can accelerate compositional phase diagram construction significantly. By calculating the energies of all known compounds in a given chemical system (e.g. the lithium/iron/oxygen chemical system, Li-Fe-O), we can determine the phase diagram for that system at a temperature of $$T=0$$ K and pressure of $$P=0$$ **** atm. Furthermore, for systems comprised of predominantly solid phases open with respect to a gaseous element, approximations can be made as to the finite temperature and pressure phase diagrams.

In this section, we will describe the theory/methodology behind the calculation of compositional phase diagrams.

## Methodology

This section will discuss how to construct phase diagrams from DFT-calculated energies. This is exact process done by the Materials Project (MP) for computing formation energies, thermodynamic stability, and phase diagrams. This methodology has been implemented in Python within the **pymatgen** package. Please see [#code-pymatgen](phase-diagrams-pds.md#code-pymatgen "mention")for brief examples of how to build phase diagrams on your own.

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

To construct a phase diagram, one needs to compare the relative thermodynamic stability of phases belonging to the system using an appropriate free energy model. For an isothermal, isobaric, closed system, the relevant thermodynamic potential is the Gibbs free energy, $$G$$, which can be expressed as a Legendre transform of the enthalpy, $$H$$, and internal energy, $$E$$, as follows:

$$
\eqalign{G(T,P,N_{Li},N_{Fe},N_{O}) &= H(T,P,N_{Li},N_{Fe},N_{O}) - TS(T,P,N_{Li},N_{Fe},N_{O})\cr &= \eqalign{E(T,P,N_{Li},N_{Fe},N_{O}) + PV(T,P,N_{Li},N_{Fe},N_{O})\cr - TS(T,P,N_{Li},N_{Fe},N_{O}}}
$$

where $$T$$ is the temperature of the system, $$S$$ is the entropy of the system, $$P$$ is the pressure of the system, $$V$$ is the volume of the system, and $$N_i$$ is the number of atoms of species $$i$$ in the system.

For systems comprising primarily of condensed phases, the $$PV$$ term can be neglected and at 0K, the expression for $$G$$ simplifies to just $$E$$. Normalizing $$E$$ with respect to the total number of particles in the system, we obtain $$\bar{E}(0,P,x_{Li},x_{Fe},x_O)$$. By taking the convex hull [\[2\]](phase-diagrams-pds.md#references) of $$E$$ for all phases belonging to the M-component system and projecting the stable nodes into the $$(M-1)$$- dimension composition space, one can obtain the 0 K phase diagram for the closed system at constant pressure. The convex hull of a set of points is the smallest convex set containing the points. For instance, to construct a 0 K, closed $$Li-Fe-O$$ system phase diagram, the convex hull is taken on the set of points in $$(\bar{E},x_{Li},x_{Fe})$$ space with $$x_O$$ being related to the other composition variables by $$x_O = 1 - x_{Li} - x_{Fe}$$.

#### Evaluating thermodynamic stability

![Figure 2: Illustration of various thermodynamic stability metrics, reproduced from Bartel \[1\].](../../../.gitbook/assets/10853\_2022\_6915\_Fig1\_HTML.webp)

_Figure 2_ is an example of a calculated binary A-X phase diagram at 0 K and 0 atm. Binary phase diagrams show the complete convex hull for the system, where the y-axis is the formation energy per atom and the x-axis is the composition.

The blue lines show the convex hull construction, which connects stable phases (circles). Unstable phases will always appear above the convex hull line (squares); one measure of the thermodynamic stability of an arbitrary compound is its distance from the convex hull line ($$\Delta E_d$$), which predicts the decomposition energy of that phase into the most stable phases.

## **Accuracy of Calculated Phase Diagrams**

In general, we can expect that compositional phase diagrams comprising of predominantly solid phases to be reproduced fairly well by our calculations. However, it should be noted that there are inherent limitations in accuracy in the DFT calculated energies. Furthermore, our calculated phase diagrams are at 0 K and 0 atm, and differences with non-zero temperature phase diagrams are to be expected.

For grand potential phase diagrams, further approximations are made as to the entropic contributions [\[2\]](phase-diagrams-pds.md#references). They are therefore expected to be less accurate, but nonetheless provide useful insights on general trends.

## Code (pymatgen)

While the Materials Project website has a phase diagram app ([https://materialsproject.org/phasediagram](https://materialsproject.org/phasediagram)), and `PhaseDiagram` objects can also be obtained directly from the API ([#phase-diagram](../../../downloading-data/using-the-api/examples.md#phase-diagram "mention")), two code snippets are provided below that show how to use the API and pymatgen to construct and plot your own phase diagrams with Python.&#x20;

#### GGA/GGA+U

Constructing mixed GGA/GGA+U phase diagrams **can be done directly with the corrected `ComputedStructureEntry` objects from the API.**&#x20;

```python
from mp_api.client import MPRester
from pymatgen.analysis.phase_diagram import PhaseDiagram, PDPlotter

with MPRester("your_api_key") as mpr:

    # Obtain only corrected GGA and GGA+U ComputedStructureEntry objects
    entries = mpr.get_entries_in_chemsys(elements=["Li", "Fe", "O"], 
                                         additional_criteria={"thermo_types": ["GGA_GGA+U"]}) 
    # Construct phase diagram
    pd = PhaseDiagram(entries)
    
    # Plot phase diagram
    PDPlotter(pd).get_plot()
```

#### GGA/GGA+U/R2SCAN

Constructing a mixed GGA/GGA+U/R2SCAN phase diagram **requires corrections to be reapplied locally.** This is because the corrected `ComputedStructureEntry` object obtained from the thermodynamic data endpoint of the API is from the home chemical system phase diagram for the material (i.e. `Si-O` for SiO2, or `Li-Fe-O` for Li2FeO3).&#x20;

**Unlike the previous GGA/GGA+U only mixing scheme, the updated scheme does not guarantee the same correction to an entry in phase diagrams for different chemical system.** In other words, the energy correction applied to the entry for silicon (mp-149) in the Si-O phase diagram is not guaranteed to be the same for the one in the Si-O-P phase diagram.&#x20;

For more details on the correction scheme and its logic, see the Energy Corrections section or the original publication [\[4\]](phase-diagrams-pds.md#references).

```python
from mp_api.client import MPRester
from pymatgen.analysis.phase_diagram import PhaseDiagram, PDPlotter
from pymatgen.entries.mixing_scheme import MaterialsProjectDFTMixingScheme

with MPRester("your_api_key") as mpr:

    # Obtain GGA, GGA+U, and r2SCAN ComputedStructureEntry objects
    entries = mpr.get_entries_in_chemsys(elements=["Li", "Fe", "O"], 
                                         additional_criteria={"thermo_types": ["GGA_GGA+U", "R2SCAN"]}) 
    
    # Apply corrections locally with the mixing scheme
    scheme = MaterialsProjectDFTMixingScheme()
    corrected_entries = scheme.process_entries(entries)
    
    # Construct phase diagram
    pd = PhaseDiagram(corrected_entries)
    
    # Plot phase diagram
    PDPlotter(pd).get_plot()
```

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

\[4] Kingsbury, R.S., Rosen, A.S., Gupta, A.S. _et al._ A flexible and scalable scheme for mixing computed formation energies from different levels of theory. _npj Comput Mater_ 8, 195 (2022). [https://doi.org/10.1038/s41524-022-00881-w](https://doi.org/10.1038/s41524-022-00881-w)

