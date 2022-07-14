---
description: >-
  Details of calculation parameters for the density functional theory (DFT)
  calculation results contained in the Materials Project (MP) database.
---

# Calculation Details

## Parameters and Convergence

We use density functional theory as implemented in the Vienna Ab Initio Simulation Package (VASP) software [\[1\]](./#references) to evaluate the total energy of compounds. For the exchange-correlational functional, we employ a mix of Generalized Gradient Approximation (GGA) and GGA+_U_, as described [later in this manual](../materials-methodology/thermodynamic-stability/#gga-gga+u-mixing-corrections). We use the Projector Augmented Wave (PAW) method for modeling core electrons with an energy cutoff of 520 eV. This cutoff corresponds to 1.3 times the highest cutoff recommended among all the pseudopotentials we use (more details can be found in the Pseudopotentials Choice manual). All calculations are performed at 0K and 0atm. All computations are performed with spin polarization on and with magnetic ions in a high-spin ferromagnetic initialization (the system can of course relax to a low spin state during the DFT relaxation). We used a k-point mesh of 1000/(number of atoms in the cell) for all computations, the Monkhorst-Pack method for the k-point choices (but $$\Gamma$$-centered for hexagonal cells) and the tetrahedron method to perform the k-point integration. Pymatgen could change those default parameters if they are not adequate for the computation (e.g., switch to another k-point integration scheme). Some details of our calculation method can be found in ref [\[2\]](./#references); however, the Materials Project has updated many parameters as documented throughout the Methodology sections.

### Crystal structures

We use input structures from the Inorganic Crystal Structure Database (ICSD), [\[3\]](./#references) and relax all cell and atomic positions in our calculation two times in consecutive runs. When multiple crystal structures are present for a single chemical composition, we attempt to evaluate all unique structures as determined by an affine mapping technique. [\[4\]](./#references)

### Total energy convergence

We currently employ a k-point mesh of 1000 per reciprocal atom (pra). We performed a convergence test of total energy with respect to k-point density and convergence energy difference for a subset of chemically diverse compounds for a previous parameter set, which employed a smaller k-point mesh of 500 pra. Using a 500 pra k-point mesh, the numerical convergence for most compounds tested was within 5 meV/atom, and 96% of compounds tested were converged to within 15 meV/atom. Results for the new parameter set will be better due to the denser k-point mesh employed. Convergence will depend on chemical system; for example, oxides were generally converged to less than 1 meV/atom. [\[5\]](./#references)

### Structure convergence

The energy difference for ionic convergence is set to 0.0005 \* natoms in the cell. Data on expected accuracy on cell volumes can be found in a previous paper. [\[2\]](./#references) We have found these parameters to yield well-converged structures in most instances; however, if the structures are to be used for further calculations that require strictly converged atomic positions and cell parameters (e.g. elastic constants, phonon modes, etc.), we recommend that users re-optimize the structures with tighter cutoffs or in force convergence mode.



More detailed information on GGA_+U_ and pseudopotentials used see the two subsections:

{% content-ref url="gga+u-calculations.md" %}
[gga+u-calculations.md](gga+u-calculations.md)
{% endcontent-ref %}

{% content-ref url="../../apps/mof-explorer/calculation-parameters/psuedopotentials.md" %}
[psuedopotentials.md](../../apps/mof-explorer/calculation-parameters/psuedopotentials.md)
{% endcontent-ref %}

## References

\[1]: Kresse, G. & Furthmuller, J., 1996. Efficient iterative schemes for ab initio total-energy calculations using a plane-wave basis set. Physical Review B, 54, pp.11169-11186.

\[2]: A. Jain, G. Hautier, C. Moore, S.P. Ong, C.C. Fischer, T. Mueller, K.A. Persson, G. Ceder., A High-Throughput Infrastructure for Density Functional Theory Calculations, Computational Materials Science. vol. 50 (2011) 2295-2310.

\[3]: G. Bergerhoff, The inorganic crystal-structure data-base, Journal Of Chemical Information and Computer Sciences. 23 (1983) 66-69.

\[4]: R. Hundt, J.C. Schön, M. Jansen, CMPZ - an algorithm for the efficient comparison of periodic structures, Journal Of Applied Crystallography. 39 (2006) 6-16.

\[5]: L. Wang, T. Maxisch, G. Ceder, Oxidation energies of transition metal oxides within the GGA+U framework, Physical Review B. 73 (2006) 1-6.
