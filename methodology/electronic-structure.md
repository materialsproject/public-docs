# Electronic Structure

## Calculation Details

A relaxed structure associated with the canonical data in the `entries` field of a material data entry is used to run both uniform and line-mode NSCF calculations with the same functional (and U if any). Currently, only GGA (PBE) and GGA+U DOS and band structures are available from the database.

We first run a static (SCF) calculation with a uniform (Monkhorst Pack or $$\Gamma$$-centered for hexagonal systems) k-point grid determined by the standard `MPStaticSet` input set in pymatgen. The charge density is extracted from this and used for the subsequent uniform and line-mode NSCF calculations. The parameters for both of these are determined by the `MPNonSCFSet` input set in pymatgen. For more details, see the band structure workflow in [atomate](https://atomate.org/).

### Line-mode Band Structure

The line-mode NSCF calculation is run with k-points chosen along high-symmetry lines within the Brillouin zone of the material. Currently, three conventions for choosing this k-path are used, and follow the methodologies by Curtarolo et al. [\[1\]](electronic-structure.md#references), Hinuma et al. [\[2\]](electronic-structure.md#references), and Munro et al. [\[3\]](electronic-structure.md#references) Code for generating the k-paths can be found within [pymatgen](https://pymatgen.org/pymatgen.symmetry.bandstructure.html?highlight=highsymmkpath#pymatgen.symmetry.bandstructure.HighSymmKpath).

The Setyawan-Curtarolo band structure data is displayed on the website by default with full lines for spin-up and dashed lines for spin-down. For insulators, the band gap is computed according to the band structure. The nature of the gap (direct or undirect) as well as the k-points involved in the band gap transition are displayed. The VBM and CBMs are displayed for insulators as well by purple dots. Note that the website might not show all bands included in the calculation. These can be obtained by by downloaded the band structure data from the [API](../downloading-data/using-the-api/).

### Density of States (DOS)

The DOS displayed on the website shows the total DOS, and elemental projections by default. However, total orbital and elemental orbital projections are also calculated and available from the [API](../downloading-data/using-the-api/). Please note that the DOS data and line-mode band structure may not completely agree on all derived properties such as the band-gap due to k-point grid differences. For instance, the uniform k-point grid used to calculate the DOS might not include some specific k-points along one of the high-symmetry lines, while the line-mode band structure will.

### Material Band Gap

The band gap listed for a given material is chosen from one of its calculations. The current calculation hierarchy is as follows:

**Density of States > Line-mode Band Structure > Static (SCF) > Optimization**

## Accuracy of Band Structures

**Note**: T_he term 'band gap' in this section generally refers to the fundamental gap, not the optical gap. The difference between these quantities is reported to be small in semiconductors but significant in insulators._ [\[4\]](electronic-structure.md#references)

![](../.gitbook/assets/band\_gaps.png)

&#x20;_Figure 1: Experimental versus computed band gaps for 237 compounds in an internal test. The computed gaps are underestimated by an average factor of 1.6, and the residual error even after accounting for this shift is significant (MAE of 0.6 eV). We thank M. Chan for her assistance in compiling this data._

Density functional theory is formulated to calculate ground state properties. Although the band structure involves excitations of electrons to unoccupied states, the Kohn-Sham energies used in solving the DFT equations are often interpreted to correspond to electron energy levels in the solid.

The correspondence between the Kohm-Sham eigenvalues computed by DFT and true electron energies is theoretically valid only for the highest occupied electron state. The Kohn-Sham energy of this state matches the first ionization energy of the material, given an exact exchange-correlation functional. However, for other energies, there is no guarantee that Kohn-Sham eigenvalues will correspond to physical observables.

Despite the lack of a rigorous theoretical basis, the DFT band structure does provide useful information. In general, band dispersions predicted by DFT are reported to match experimental observations; one small test of band dispersion accuracy found that errors ranged from 0.1 to about 0.4 eV.[\[5\]](electronic-structure.md#references) However, predicted band gaps are usually severely underestimated. Therefore, a common way to interpret DFT band structures is to apply a 'scissor' operation whereby the conduction bands are shifted in energy by a constant amount so that the band gap matches known experimental observations.

### Band gaps

In general, band gaps computed with common exchange-correlation functionals such as the LDA and GGA are severely underestimated. Typically the disagreement is reported to be around 50% in the literature. Some internal testing by the Materials Project supports these statements; typically, we find that band gaps are underestimated by about 40% (Figure 1). We additionally find that several known insulators are predicted to be metallic.

#### Origin of band gap error and improving accuracy

The errors in DFT band gaps obtained from calculations can be attributed to two sources: 1. Approximations employed to the exchange correlation functional 2. A derivative discontinuity term, originating from the true density functional being discontinuous with the total number of electrons in the system.

Of these contributions, (2) is generally regarded to be the larger and more important contribution to the error. It can be partly addressed by a variety of techniques such as the GW approximation but typically at high computational cost.

Strategies to improve band gap prediction at moderate to low computational cost now been developed by several groups, including Chan and Ceder (delta-sol),[\[6\]](electronic-structure.md#references) Heyd et al. (hybrid functionals) [\[7\]](electronic-structure.md#references), and Setyawan et al. (empirical fits) [\[8\]](electronic-structure.md#references). (These references also contain additional data regarding the accuracy of DFT band gaps.) The Materials Project may employ such methods in the future in order to more quantitatively predict band gaps. For the moment, computed band gaps should be interpreted with caution.

## Citation

To cite the calculation methodology, please reference the following works:

1. A. Jain, G. Hautier, C. Moore, S.P. Ong, C.C. Fischer, T. Mueller, K.A. Persson, G. Ceder., A High-Throughput Infrastructure for Density Functional Theory Calculations, Computational Materials Science, vol. 50, 2011, pp. 2295-2310. [DOI:10.1016/j.commatsci.2011.02.023](https://dx.doi.org/10.1016/j.commatsci.2011.02.023)

## Authors

1. Anubhav Jain
2. Shyue Ping Ong
3. Geoffroy Hautier
4. Charles Moore
5. Jason Munro

## References

\[1]: W. Setyawan, S. Curtarolo, High-throughput electronic band structure calculations: Challenges and tools, Computational Materials Science 2010, 49, 299-312.

\[2]: Y. Hinuma, P. Giovanni, Y. Kumagai, F. Oba, I. Tanaka, Band structure diagram paths based on crystallography Computational Materials Science 2017, 128, 140-184.

\[3]: J.M. Munro, K. Latimer, M.K. Horton, S. Dwaraknath, K.A. Persson, An improved symmetry-based approach to reciprocal space path selection in band structure calculations, npj Computarional Materials 2020, 6, 112.

\[4]: E.N. Brothers, A.F. Izmaylov, J.O. Normand, V. Barone, G.E. Scuseria, Accurate solid-state band gaps via screened hybrid electronic structure calculations., The Journal of Chemical Physics. 129 (2008)

\[5]: R. Godby, M. Schluter, L.J. Sham, Self-energy operators and exchange-correlation potentials in semiconductors, Physical Review B. 37 (1988).

\[6]: M. Chan, G. Ceder, Efficient Band Gap Predictions for Solids, Physical Review Letters 19 (2010)

\[7]: J. Heyd, J.E. Peralta, G.E. Scuseria, R.L. Martin, Energy band gaps and lattice parameters evaluated with the Heyd-Scuseria-Ernzerhof screened hybrid functional, Journal of Chemical Physics 123 (2005)

\[8]: W. Setyawan, R.M. Gaume, S. Lam, R. Feigelson, S. Curtarolo, High-throughput combinatorial database of electronic band structures for inorganic scintillator materials., ACS Combinatorial Science. (2011).
