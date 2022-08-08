---
description: >-
  How aqueous stability (Pourbaix) diagrams are calculated and plotted on the
  Materials Project (MP) website.
---

# Aqueous Stability (Pourbaix)

{% hint style="info" %}
If using Pourbaix functionality for scientific research, please make sure to consult the original peer-reviewed publication and the[ "Known Issues"](aqueous-stability-pourbaix.md#known-issues) section below.
{% endhint %}

### Introduction

A Pourbaix diagram, also frequently called a potential-pH diagram, or E-pH diagram, is a representation of aqueous phase electrochemical equilibria.

It is a two-dimensional representation of a three-dimensional free energy-pH-potential diagram.

In other words, it shows water-stable phases as a function of pH and potential, where, potential is defined with respect to the standard hydrogen electrode.\


Experimentally determining Pourbaix Diagrams is painstaking work, as we need not only the free energy of aqueous ions, but also that of all solid phases that a system can exist in.

The Materials Project offers a very convenient and powerful database of materials properties which has been used to generate Pourbaix diagrams in a high-throughput manner.

This manual outlines the usage of the Pourbaix App to calculate Pourbaix diagrams, and the thermodynamic formalism underlying the app.

### Thermodynamic Formalism of Pourbaix Diagrams

To calculate a Pourbaix diagram, free energies of the solid phases, and of the aqueous ions is required.

Calculating free energies of ions is tricky, and time-consuming.

To overcome this problem, a methodology utilizing experimentally measured free energies of aqueous ions and the calculated DFT energies for solid phases available in the Materials Project was developed.

\[^1] Note that the correction scheme described below is applied over and above any compatibilities/corrections which are applied to the species.

#### Referencing Energies of Aqueous Ions

Briefly, for each ion, a reference solid is chosen, and the correction term is calculated for the ion as the energy difference between the experimental and the DFT calculated energies of the reference solid.

The basic idea behind this scheme is that, if we have a reference energy for an aqueous ion which reproduces the correct dissolution for one solid, then accurate DFT solid-solid energy differences ensure that all other solids dissolve accurately with respect to that ion.

The better the solid is represented by DFT, the more transferable the reference aqueous energy becomes.

We therefore prefer to choose simple chemical systems (primarily binaries with an uncomplicated electronic structure) as representative solids.

For an aqueous ion i at standard state conditions (e.g., room temperature, atmospheric pressure, and $$10^{-6}~\textup{M}$$ concentration) using a representative solid s, we define the chemical potential as:

![ Schematic to reference experimental aqueous ion energies to DFT data.](../.gitbook/assets/Ion\_ref\_energy.png)

$$
\begin{align} \mu_{i(aq)}^0 &= \mu_{i(aq)}^{0,{\rm exp}} + \left[\Delta g_s^{0,{\rm DFT}} - \Delta g_s^{0,{\rm exp}}\right]  \ & = \mu_{i(aq)}^{0,{\rm exp}} + \Delta\mu_s^{0, {\rm DFT - exp}} \end{align}
$$

Figure 1 shows this schematically.

#### Correction for Water 

In an aqueous environment, many chemical and electrochemical reactions are enabled by the breakdown, formation, or incorporation of water molecules.

It is therefore important that the free energy of formation of water is captured accurately.

This is known accurately from experiments as $$-2.46~\textup{eV}$$.

So, at standard state, the free energy of formation of water is set as follows:

$$
\begin{align}
\mu^0_{{\rm H_2O}} &= \Delta g^{0, {\rm exp}}_{\rm H_2O}  \
 & = -2.46 eV
\end{align}
$$

#### Correction for Elemental Gaseous, and Liquid States, except $$\textup{H}_2$$

For all gaseous elements, the experimentally determined entropic contribution at 298 K is added to the DFT/corrected energy of the element as follows:

$$
\mu_i^{\rm ref} = E_i^{0, {\rm DFT}} + \Delta E_i^{\rm correction} - Ts_i^{\rm exp}
$$

This is implemented for the following elements: $$\textup{O}_2, \textup{F}_2, \textup{Cl}_2, \textup{Br}_2, \textup{Hg}$$.

#### Correction for $$\textup{H}_2$$

In an aqueous environment, $$\textup{O}_2$$ and $$\textup{H}_2$$ in their gaseous states are in equilibrium with water through the reaction

$$\textup{H}_{2(g)} + \frac{1}{2}\textup{O}_{2(g)} \longleftrightarrow \textup{H}_2\textup{O}_{(l)}$$

Hence, the hydrogen energy is corrected such that the experimental free energy of formation of $$\textup{H}_2\textup{O}$$ is reproduced.

$$
\begin{align}
\mu_{\rm H}^{\rm ref} & = \frac{1}{2}\left[g_{\rm H_2O}^0 - \mu_O^0 - \Delta g_{\rm H_2O}^{0, {\rm exp}} \right] \
& \approx \frac{1}{2}\left[ E_{\rm H_2O}^{0, {\rm DFT}} -Ts_{\rm H_2O}^0 - \mu_O^0 - \mu_{\rm H_2O}^{0, {\rm exp}} \right]
\end{align}
$$



### Electrochemical Stability of Metastable Materials

In principle, Pourbaix diagrams account for materials only at thermodynamic equilibrium, providing no insight into the electrochemical stability of metastable materials which find practical applications in many commercial applications.

However, one can compute the Gibbs free energy difference for an arbitrary material with respect to the Pourbaix stable domains as a function of pH and E, providing an electrochemical (in)stability map for this material.

For detailed information on the formalism and its applications see reference 2.\[^2]

### Literature References for Ions

The free energies of ions in the aqueous phase have been taken from standard references and recent publications:

NBS Tables: NBS Thermodynamic tables.\[^4]

M. Pourbaix (1974): Atlas of Electrochemical Equilibria in Aqueous Solutions. \[^5]

Barin Knacke Kubaschewski: Thermochemical Properties of Inorganic Substances \[^6]

Barner and Scheuerman (1978): Handbook of thermochemical data for compounds and aqueous species \[^7]

Beverskog and Puigdomenech (1997): Beverskog and Puigdomenech, Corr. Sci. (1997) \[^8]

### Known Issues

* There is a trade off in the construction of the Pourbaix diagrams given the available data, and they are currently constructed to favor accuracy of oxides, meaning that it is not uncommon to see less accurate results in the reducing (low E) region where hydrides are typically observed.
* We recently discovered an error for `IrO4[-2]` due to a typo in one of the original data sources. Prior to July 28th 2022, the charge and formula for this species were incorrectly listed in [our dataset](https://contribs.materialsproject.org/projects/ion\_ref\_data) as -1 and `IrO4[-1]`, respectively, while the energy value was correct. This error has now been corrected in the ion data used by the [new API](../downloading-data/differences-between-new-and-legacy-api.md) and [website](../changes/#website). However, note that the legacy Materials Project website and API will still have the incorrect charge for this species.

### References

\[^1]: K. A. Persson, B. Waldwick, P. Lazic, and G. Ceder, Phys. Rev. B, 85, 235438 (2012)

\[^2]: A. K. Singh, L. Zhou, A. Shinde, S. K. Suram, J. H. Montoya, D. Winston, J. M. Gregoire, K. A. Persson, Chem. Mater. 29, 10159 (2017)

\[^3]: Pourbaix Diagrams for Multielement Systems, Thompson, W. T., Kaye, M. H., Bale, C. W. and Pelton, A. D. (2011), in Uhlig's Corrosion Handbook, Third Edition (ed R. W. Revie), John Wiley & Sons, Inc., Hoboken, NJ, USA.

\[^4]: NBS Technical Note 270-1 to 270-8. D. D. Wagman et. al, U. S. Department of Commerce (1973)

\[^5]: Atlas of Electrochemical Equilibria in Aqueous Solutions, M. Pourbaix,, NACE (1974)

\[^6]: Thermochemical Properties of Inorganic Substances, I. Barin, O. Knacke, and O. Kubaschewski, Springer-Verlag, Berlin (1977)

\[^7]: Handbook of thermochemical data for compounds and aqueous species, H. E. Barner, and R. V. Scheuerman, Wiley, New York, 1978

\[^8]: Revised Pourbaix diagrams for Ni at 25-300^oC, B. Beverskog and I. Puigdomenech, Corr. Sci., 39, 969-980 (1997)

\


### Authors

* Sai Jayaraman
* Arunima K. Singh
* Rebecca Stern
* Eric Sivonxay
* Ryan Kingsbury
