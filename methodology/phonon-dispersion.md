# Phonon Dispersion

## Introduction

A phonon is a collective excitation of a set of atoms in condensed matter. These excitations can be decomposed into different modes, each being associated with an energy that corresponds to the frequency of the vibration. The different energies associated with each vibrational mode constitute the phonon vibrational spectra (or phonon band structure). The vibrational spectra of materials play an important role in physical phenomena such as thermal conductivity, superconductivity, ferroelectricity and carrier thermalization.

There are different methods to calculate the vibrational spectra from first-principles using the density functional theory formalism (DFT). It can be obtained from the Fourier transform of the trajectories of the atoms on a molecular dynamics run, from finite-differences of the total energy with respect to atomic displacements or directly from density functional perturbation theory (DFPT). The latter method is the one used in the calculations on the Materials project page.

## Formalism

In the density functional perturbation theory formalism the derivatives of the total energy with respect to a perturbation are directly obtained from the self-consistency loop [\[1\]](phonon-dispersion.md#references) For a generic point **q** in the Brillouin zone the phonon frequencies $$\omega_{\mathbf{q},m}$$ and eigenvectors $$U_m(\mathbf{q}\kappa'\beta)$$ are obtained by solving of the generalized eigenvalue problem

$$
\sum_{\kappa'\beta}\widetilde{C}_{\kappa\alpha,\kappa'\beta}(\mathbf{q})U*m(\mathbf{q}\kappa'\beta) = M_{\kappa}\omega^2_{\mathbf{q},m}U_m(\mathbf{q}\kappa\alpha)
$$

where $$\kappa$$ labels the atoms in the cell, $$\alpha$$ and $$\beta$$ are cartesian coordinates and $$\widetilde{C}_{\kappa\alpha,\kappa'\beta}(\mathbf{q})$$ are the interatomic force constants in reciprocal space, which are related to the second derivatives of the energy with respect to atomic displacements. These values have been obtained by performing a Fourier interpolation of those calculated on a regular grid of **q**-points obtained with DFPT.

## Thermodynamic properties

The vibrational density of states $$g(\omega)$$ is obtained from an integration over the full Brillouin zone

$$
g(\omega) = \frac{1}{3nN}\sum_{\mathbf{q},m}\delta(\omega-\omega_{\mathbf{q},m})
$$

where $$n$$ is the number of atoms per unit cell and $$N$$ is the number of unit cells. The expressions for the Helmholtz free energy $$\Delta F$$, the phonon contribution to the internal energy $$\Delta E_{\text{ph}}$$, the constant-volume specific heat $$C_v$$ and the entropy $$S$$ can be obtained in the harmonic approximation [\[2\]](phonon-dispersion.md#references)

$$
\Delta F = 3nNk*BT\int_{0}^{\omega_L}\text{ln}\left(2\text{sinh}\frac{\hbar\omega}{2k_BT}\right)g(\omega)d\omega
$$

$$
\Delta E_{\text{ph}} = 3nN\frac{\hbar}{2}\int_{0}^{\omega_L}\omega\text{coth}\left(\frac{\hbar\omega}{2k_BT}\right)g(\omega)d\omega
$$

$$
C*v = 3nNk_B\int_{0}^{\omega_L}\left(\frac{\hbar\omega}{2k_BT}\right)^2\text{csch}^2\left(\frac{\hbar\omega}{2k_BT}\right)g(\omega)d\omega
$$

$$
S = 3nNk*B\int_{0}^{\omega_L}\left(\frac{\hbar\omega}{2k_BT}\text{coth}\left(\frac{\hbar\omega}{2k_BT}\right) - \text{ln}\left(2\text{sinh}\frac{\hbar\omega}{2k_BT}\right)\right)g(\omega)d\omega
$$

where $$k_B$$ is the Boltzmann constant and $$\omega_L$$ is the largest phonon frequency.

## Calculation details

All the DFT and DFPT calculations are performed with the ABINIT software package [\[3,4\]](phonon-dispersion.md#references).

The PBEsol [\[5\]](phonon-dispersion.md#references) semilocal generalized gradient approximation exchange-correlation functional (XC) is used for the calculations. This functional is proven to provide accurate phonon frequencies compared to experimental data [\[6\]](phonon-dispersion.md#references). The pseudopotentials are norm-conserving [\[7\]](phonon-dispersion.md#references) and taken from the pseudopotentials table Pseudo-dojo version 0.3 [\[8\]](phonon-dispersion.md#references).

The plane wave cutoff is chosen based on the hardest element for each compound, according to the values suggested in the Pseudo-dojo table. The Brillouin zone is sampled using equivalent **k**-point and **q**-point grids that respect the symmetries of the crystal with a density of approximately 1500 points per reciprocal atom and the **q**-point grid is always $$\Gamma$$-centered \[^9].

All the structures are relaxed with strict convergence criteria, i.e. until all the forces on the atoms are below $$10^{-6}$$ Ha/Bohr and the stresses are below $$10^{-4}$$ Ha/Bohr$$^3$$.

The primitive cells and the band structures are defined according to the conventions of Setyawan and Curtarolo [\[10\]](phonon-dispersion.md#references).

## Citation

Guido Petretto, Shyam Dwaraknath, Henrique P. C. Miranda, Donald Winston, Matteo Giantomassi, Michiel J. van Setten, Xavier Gonze, Kristin A. Persson, Geoffroy Hautier, Gian-Marco Rignanese, _High-throughput density functional perturbation theory phonons for inorganic materials_, **Scientific Data**, 5, 180065 (2018). [doi:10.1038/sdata.2018.65](https://doi.org/10.1038/sdata.2018.65)

## References

\[1]: Gonze, X. & Lee, C. Dynamical matrices, Born effective charges, dielectric permittivity tensors, and interatomic force constants from density functional perturbation theory. Phys. Rev. B 55, 10355–10368 (1997)

\[2]: C. Lee & X. Gonze, Ab initio calculation of the thermodynamic properties and atomic temperature factors of SiO2 α-quartz and stishovite. Phys. Rev. B 51, 8610 (1995)

\[3]: Gonze, X. et al. First-principles computation of material properties: the Abinit software project. Computational Materials Science 25, 478 – 492 (2002)

\[4]: Gonze, X. et al. ABINIT: First-principles approach to material and nanosystem properties. Computer Physics Communications 180, 2582 – 2615 (2009)

\[5]: Perdew, J. P. et al. Restoring the density-gradient expansion for exchange in solids and surfaces. Phys. Rev. Lett. 100, 136406 (2008)

\[6]: He, L. et al. Accuracy of generalized gradient approximation functionals for density-functional perturbation theory calculations. Phys. Rev. B 89, 064305 (2014)

\[7]: Hamann, D. R. Optimized norm-conserving Vanderbilt pseudopotentials. Phys. Rev. B 88, 085117 (2013)

\[8]: van Setten, M., Giantomassi, M., Bousquet, E., Verstraete, M.J., Hamann, D.R., Gonze, X. & Rignanese, G.-M., et al. The PseudoDojo: Training and grading a 85 element optimized norm-conserving pseudopotential table (2018). Computer Physics Communications 226, 39.

\[9]: Petretto, G., Gonze, X., Hautier, G. & Rignanese, G.-M. Convergence and pitfalls of density functional perturbation theory phonons calculations from a high-throughput perspective. Computational Materials Science 144, 331 – 337 (2018)

\[10]: Setyawan, W. & Curtarolo, S. High-throughput electronic band structure calculations: Challenges and tools. Computational Materials Science 49, 299 – 312 (2010)
