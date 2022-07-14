# Dielectric Constants

## Introduction

A dielectric is a material that can be polarized by an applied electric field. This limits the dielectric effect to materials with a non-zero band gap. The mathematical description of the dielectric effect is a tensor constant of proportionality that relates an externally applied electric field to the field within the material. Along with the elastic and piezoelectric tensors, the dielectric tensor provides all the information necessary for the solution of the constitutive equations in applications where electric and mechanical stresses are coupled.

The dielectric tensors from the Materials Project (MP) are calculated from first principles Density Functional Perturbation Theory (DFPT) [\[1\]](dielectricity.md#references) and are approximated as the superimposed effect of an electronic and ionic contribution. From the full piezoelectric tensor, several properties are derived such as the refractive index and potential for ferroelectricity. Just as with the piezoelectric and elastic constants, multiple consistency checks are performed on all the calculated dielectric data to ensure its reliability and accuracy.

## Formalism

Formally, the dielectric tensor ε relates the externally applied electric field to the field within the material and can be defined as:

$$
E_i=\sum_j \epsilon^{-1}_{ij}E_{0j}
$$

where $$E$$ is the electric field inside the material and $$E_{0}$$is the externally applied electric field. the indices $$i,j$$ refer to the direction in space and take the values: $$1,2,3$$. The dielectric tensor can be split in the ionic ($$\epsilon^0$$) and electronic ($$\epsilon^\infty$$) contributions:

$$
\epsilon_{ij}=\epsilon_{ij}^0+\epsilon_{ij}^\infty
$$

Here, we consider only the response of non-zero band gap materials to time-invariant fields. In the hypothetical case that a material does not respond at all to the external field, $$\epsilon_{ij}^\infty$$ would be equal to the identity tensor and $$\epsilon_{ij}^0$$ would be zero. In fact, materials with zero ionic contribution do exist. In general, for $$\epsilon_{ij}^0$$ to be non-zero, compounds need to have at least 2 atoms per primitive cell, each having a different atomic charge. The dielectric tensor is symmetric and respects all the symmetry operations of the corresponding point group. This limits the number of independent elements in the tensor to a minimum of 1 and a maximum of 6 depending on the crystal symmetry.

The dielectric response calculated herein corresponds to that of a single crystal. In polycrystalline samples, grains are oriented randomly and hence, the actual response will be different. Generally, the dielectric response varies with the frequency of the applied external field however here, we consider the static response (i.e., the response at constant electric fields or the long wavelength limit). Since the ionic contribution vanishes at high frequencies, our results can be used to obtain an estimate of the refractive index, n, at optical frequencies and far from resonance effects using the well known formula: [\[2\]](dielectricity.md#references)

$$
n=\sqrt{\epsilon^\infty_{poly}}
$$

where $$\epsilon_{poly}^\infty$$ is the average of the eigenvalues of the electronic contribution to the dielectric tensor. It should be noted this equation for the refractive index assumes the material is non-magnetic.&#x20;

## DFT Parameters

The initial set of 1,056 dielectric tensors were calculated using the Vienna Ab-Initio Simulation Package [\[3-6\]](dielectricity.md#references) (VASP version 5.3.4) combined with the Generalized Gradient Approximation GGA/PBE[\[7,8\]](dielectricity.md#references)+U[\[9,10\]](dielectricity.md#references) exchange-correlation functional and Projector Augmented Wave pseudopotentials [\[11,12\]](dielectricity.md#references). The U values are energy corrections that address the spurious self-interaction energy introduced by GGA. Here, we used U values for d orbitals only that were fitted to experimental binary formation enthalpies using Wang et al. [\[13\]](dielectricity.md#references) method. The full list of U values used, can be found in ref.[\[10\]](dielectricity.md#references). The k-point density was set at 3,000 per reciprocal atom and the plane wave energy cut-off at 600 eV (ref. 4). For detailed information on the calculation of the dielectric tensor within the DFPT framework we refer to Baroni et al. [\[14,15\]](dielectricity.md#references) and Gonze & Lee [\[16\]](dielectricity.md#references).

Piezoelectricity calculations use the same DFPT methodology with a tighter parameter set to achieve convergence. As a result, the dielectric tensor is already converged in these calculations and is reported for any non-centrosymmetric material, not in the initial dataset of dielectrics.

## Benchmarking

We see that in most cases, it is possible to predict the dielectric constant of materials with a relative deviation of less than +/−25% from experimental values at room temperature. Including local field effects gives the smallest mean absolute relative deviation ( MARD= 16.2 % for GGA). Furthermore, we note a tendency to overestimate rather than underestimate the dielectric constant relative to experiments, which is a well-known effect of DFPT [\[17,18,19\]](dielectricity.md#references) for the electronic contribution. Although it has often been related to the band gap underestimation problem of DFT, DFPT is a ground state theory and hence, the dielectric constant should, in principle, be described exactly [\[20\]](dielectricity.md#references). In fact, as described by various authors, the problem is likely linked to the exchange-correlation functional [\[21-26\]](dielectricity.md#references). Specifically, the exchange correlation functional has been found to depend on polarization but the actual dependence formula is, unfortunately, not known [\[27,28\]](dielectricity.md#references). Additionally, the validity of GGA depends on the charge density varying slowly—an assumption that may be broken when an external electric field is applied [\[30\]](dielectricity.md#references).&#x20;

## Citation

To cite the dielectric properties within the Materials Project, please reference the following work:

* Benchmarking density functional perturbation theory to enable high-throughput screening of materials for dielectric constant and refractive index. Ioannis Petousis, Wei Chen, Geoffroy Hautier, Tanja Graf, Thomas D. Schladt, Kristin A. Persson, and Fritz B. Prinz. Phys. Rev. B 93(11). [DOI:10.1103/PhysRevB.93.115151](https://doi.org/10.1103/PhysRevB.93.115151)
* High-throughput screening of inorganic compounds for the discovery of novel dielectric and optical materials. Ioannis Petousis, David Mrdjenovich, Eric Ballouz, Miao Liu, Donald Winston, Wei Chen, Tanja Graf, Thomas D. Schladt, Kristin A. Persson, and Fritz B. Prinz. Scientific Data 4. [DOI:10.1038/sdata.2016.134](https://doi.org/10.1038/sdata.2016.134)

These papers present the results of our dielectric constant-calculations for the first batch of 1,056 compounds. Our DFT-parameters, the workflow, the workflow filters used for detecting anomalies in the calculations and comparison to experiments are described in detail.

## Authors

1. Shyam Dwaraknath
2. Ioannis Petousis

## References

\[1]: Baroni, Giannozzi S. P. and Testa, A. Phys. Rev. Lett. 58, 1861 (1987)

\[2]: Petousis I. et al. Benchmarking of the density functional perturbation theory to enable the high-throughput screening of materials for the dielectric constant and refractive index. Phys. Rev. B 93, 115151 (2016).

\[3]: Kresse G. & Hafner J. Ab initio molecular dynamics for liquid metals. Phys. Rev. B 47, 558–561 (1993).

\[4]: Kresse G. & Hafner J. Ab initio molecular-dynamics simulation of the liquid-metal-amorphous-semiconductor transition in germanium. Phys. Rev. B 49, 14251 (1994).

\[5]: Kresse G. & Furthmüller J. Efficiency of ab-initio total energy calculations for metals and semiconductors using a plane-wave basis set. Comp. Mater. Sci. 6, 15–50 (1996).

\[6]: Kresse G. & Furthmüller J. Efficient iterative schemes for ab initio total-energy calculations using a plane-wave basis set. Phys. Rev. B 54, 11169 (1996).

\[7]: Perdew J. P., Burke K. & Ernzerhof M. Generalized gradient approximation made simple. Phys. Rev. Lett. 77, 3865 (1996).

\[8]: Perdew J. P., Burke K. & Ernzerhof M. Generalized gradient approximation made simple \[Phys. Rev. Lett. 77, 3865 (1996)]. Phys. Rev. Lett. 78, 1396 (1997).

\[9]: Dudarev S. L., Botton G. A., Savrasov S. Y., Humphreys C. J. & Sutton A. P. Electron-energy-loss spectra and the structural stability of nickel oxide: An LSDA+U study. Phys. Rev. B 57, 1505 (1998).

\[10]: Jain A. et al. A high-throughput infrastructure for density functional theory calculations. Comp. Mater. Sci. 50, 8 2295 (2011).

\[11]: Blöchl P. E. Projector augmented-wave method. Phys. Rev. B 50, 17953 (1994).

\[12]: Kresse G. & Joubert D. From ultrasoft pseudopotentials to the projector augmented-wave method. Phys. Rev. B 59, 1758 (1999).

\[13]: Wang L., Maxisch T. & Ceder G. Oxidation energies of transition metal oxides within the GGA+ U framework. Phys. Rev. B 73, 195107 (2006).

\[14]: Baroni S., Giannozzi P. & Testa A. Elastic constants of crystals from linear-response theory. Phys. Rev. Lett. 59, 2662 (1987).

\[15]: Baroni S., de Gironcoli S., Dal Corso A. & Giannozzi P. Phonons and related crystal properties from density-functional perturbation theory. Rev. Mod. Phys. 73, 515 (2001).

\[16]: Gonze X. & Lee C. Dynamical matrices, born effective charges, dielectric permittivity tensors, and interatomic force constants from density-functional perturbation theory. Phys. Rev. B 55, 10355 (1997).

\[17]: N. Marzari and D. J. Singh, Phys. Rev. B 62, 12724 (2000). \[^18]: A. Dal Corso, S. Baroni, and R. Resta, Phys. Rev. B 49, 5323 (1994).

\[19]: F. Kootstra, P. L. de Boeij, and J. G. Snijders, Phys. Rev. B 62, 7071 (2000).

\[20]: A. Dal Corso, S. Baroni, and R. Resta, Phys. Rev. B 49, 5323 (1994).

\[21]: A. Dal Corso, S. Baroni, and R. Resta, Phys. Rev. B 49, 5323 (1994).

\[22]: V. Olevano, M. Palummo, G. Onida, and R. Del Sole, Phys. Rev. B 60, 14224 (1999).

\[23]: W. G. Aulbur, L. Jönsson, and J. W. Wilkins, Phys. Rev. B 54, 8540 (1996).

\[24]: Ph. Ghosez, X. Gonze, and R. W. Godby, Phys. Rev. B 56, 12811 (1997).

\[25]: R. Resta, Phys. Rev. Lett. 77, 2265 (1996). \[^26]: R. Resta, Phys. Rev. Lett. 78, 2030 (1997). \[27]: A. Dal Corso, S. Baroni, and R. Resta, Phys. Rev. B 49, 5323 (1994).

\[28]: W. G. Aulbur, L. Jönsson, and J. W. Wilkins, Phys. Rev. B 54, 8540 (1996).

\[29]: Ph. Ghosez, X. Gonze, and R. W. Godby, Phys. Rev. B 56, 12811 (1997).

\[30]: V. Olevano, M. Palummo, G. Onida, and R. Del Sole, Phys. Rev. B 60, 14224 (1999).
