---
description: Details on the GGA/GGA+U/r2SCAN mixing scheme corrections
---

# GGA/GGA+U/r2SCAN Mixing

An updated energy correction scheme [\[1\]](gga-gga+u-r2scan-mixing.md#references) is used to allow for the mixing of GGA, GGA+U, and r2SCAN calculations. This is constructed by considering all electronic energies to be the sum of a reference energy, and a relative energy. The reference energy ($$E_{ref}$$) for each functional is defined as the ([empirically corrected](anion-and-gga-gga+u-mixing.md)) electronic energy of the GGA(+U) ground-state structure at each point in composition space. The energy of a material associated with either functional can then be expressed as a difference relative to a specific reference energy ($$\Delta E_{ref}$$). The formation energy of a material is calculated in the usual way by subtracting the electronic energies of the elemental endpoints in each respective functional. It should be noted that $$\Delta E_{ref}$$ is calculated from the differences in polymorph energies, and consequently does not depend on the elemental endpoint energies. While the updated mixing scheme is similar to the previous scheme involving only GGA and GGA+U calculations, it extends the approach to be amenable to any two functionals without relying on pre-fitted energy correction parameters.

### Mixing Rules

The two rules used to construct mixed GGA/GGA+U/r2SCAN phase diagrams are as follows:

1. Start with a GGA(+U) convex energy hull. Replace GGA(+U) energies with r2SCAN energies by adding their $$\Delta E_{ref}$$ to the corresponding GGA(+U) reference energy.&#x20;
2. Construct the convex energy hull using formation energy calculated using r2SCAN energies, only when r2SCAN calculations exist for every reference structure (i.e. every stable GGA(+U) structure). In this case, add any missing GGA(+U) materials by adding their $$\Delta E_{ref}$$to the corresponding r2SCAN reference energy.

<figure><img src="../../../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption><p>Figure 1. Rules for mixing GGA(+U) (blue) and r2SCAN (red) energies onto a single phase diagram. (left) r2SCAN energies are placed onto the GGA(+U) hull by referencing them to the r2SCAN energy of the GGA(+U) ground state via <span class="math">\Delta E_{ref}</span>. A, B, C, ad D represent different polymophs at a single composition, and polymorph A is the ground state. (right) r2SCAN formation energies are used to build the convex hull only when there are r2SCAN calculations for every GGA(+U) ground state.</p></figcaption></figure>

For more detailed information on the mixing scheme and its benchmarks, see the original publication in Ref [\[1\]](gga-gga+u-r2scan-mixing.md#references).

## References

\[1] Kingsbury, R.S., Rosen, A.S., Gupta, A.S. _et al._ A flexible and scalable scheme for mixing computed formation energies from different levels of theory. _npj Comput Mater_ 8, 195 (2022). [https://doi.org/10.1038/s41524-022-00881-w](https://doi.org/10.1038/s41524-022-00881-w)
