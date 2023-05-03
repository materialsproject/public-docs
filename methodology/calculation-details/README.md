---
description: >-
  Details of calculation parameters for the density functional theory (DFT)
  calculation results contained in the Materials Project (MP) database.
---

# Calculation Details

We use DFT as implemented in the Vienna Ab Initio Simulation Package (VASP) software [\[1\]](./#references) to evaluate the total energy of compounds. For the exchange-correlational functional, we employ a mix of Generalized Gradient Approximation (GGA) and GGA+_U_, or a mix of GGA, GGA+U, and r2SCAN. Both mixing schemes are [described here](../materials-methodology/thermodynamic-stability/thermodynamic-stability/).  All calculations are performed at 0 K and 0 atm. All computations are performed with spin polarization on and with magnetic ions in a high-spin ferromagnetic initialization (the system can of course relax to a low spin state during the DFT relaxation). For a select number of materials, alternate spin states are searched for. Details on this can be found in the [Magnetic Properties section](../magnetic-properties.md).

Input structures are sourced from many different places, including the Inorganic Crystal Structure Database (ICSD). [\[2\]](./#references) We relax all cell and atomic positions in our calculation two times in consecutive runs. When multiple crystal structures are present for a single chemical composition, we attempt to evaluate all unique structures as determined by an affine mapping technique. [\[3\]](./#references)

More detailed information on the GGA/GGA+U and r2SCAN calculations run by the Materials Project can be found in the following two subsections:

{% content-ref url="../materials-methodology/calculation-details/gga+u-calculations/" %}
[gga+u-calculations](../materials-methodology/calculation-details/gga+u-calculations/)
{% endcontent-ref %}

{% content-ref url="../materials-methodology/calculation-details/r2scan-calculations/" %}
[r2scan-calculations](../materials-methodology/calculation-details/r2scan-calculations/)
{% endcontent-ref %}

## References

\[1]: Kresse, G. & Furthmuller, J., 1996. Efficient iterative schemes for ab initio total-energy calculations using a plane-wave basis set. Physical Review B, 54, pp.11169-11186.

\[2]: G. Bergerhoff, The inorganic crystal-structure data-base, Journal Of Chemical Information and Computer Sciences. 23 (1983) 66-69.

\[3]: R. Hundt, J.C. Schön, M. Jansen, CMPZ - an algorithm for the efficient comparison of periodic structures, Journal Of Applied Crystallography. 39 (2006) 6-16.
