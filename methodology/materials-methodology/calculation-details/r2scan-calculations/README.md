---
description: Details on r2SCAN calculations run by the Materials Project
---

# r2SCAN Calculations

Since database release `v2022.10.28` the Materials Project has incorporated metaGGA functionals into its core dataset in the form of r2SCAN calculations. Part of this includes a [new energy correction scheme](../../thermodynamic-stability/thermodynamic-stability/) that allows for the mixing of GGA, GGA+U and r2SCAN results in its thermodynamic data.

All r2SCAN data is obtained from a two-step workflow which is comprised of an initial GGA structure optimization, followed by final optimization with r2SCAN. The first step allows for the generation of an initial guess of the structure and charge density at a lower computational cost, speeding up the subsequent metaGGA calculation. More specifically, PBESol is used as the GGA functional for the first optimization step. For more details on the workflow see Ref [\[1\]](./#references).

Information regarding calculation parameters, convergence, and pseudopotential choices can also be found in the following subsections:

{% content-ref url="parameters-and-convergence.md" %}
[parameters-and-convergence.md](parameters-and-convergence.md)
{% endcontent-ref %}

{% content-ref url="pseudopotentials.md" %}
[pseudopotentials.md](pseudopotentials.md)
{% endcontent-ref %}

## References&#x20;

\[1] R. Kingsbury, A. S. Gupta, C. J. Bartel, J. M. Munro, S. Dwaraknath, M. Horton, and K. A. Persson Phys. Rev. Materials 6, 013801 (2022)
