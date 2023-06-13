---
description: >-
  Overview of how chemical potential diagrams (CPDs) are constructed and
  visualized. These are available as part of the Phase Diagram App.
---

# Chemical Potential Diagrams (CPDs)

## Introduction

The chemical potential diagram is the mathematical dual to the compositional phase diagram. To create the diagram, convex minimization is performed in energy (E) vs. chemical potential (μ) space by taking the lower convex envelope of hyperplanes. Accordingly, “points” on the compositional phase diagram become N-dimensional convex polytopes (domains) in chemical potential space.

For more information on this specific implementation of the algorithm, please cite/reference the paper below:

## Methodology

### Constructing hyperplanes

### Calculating lower convex envelope (halfspace intersection)



![Figure by Matthew McDermott.](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 3.49.34 PM.png>)

### Visualizing the chemical potential diagram

![Two dimensional (2-D) chemical potential diagram for the V-S chemical system. Energies are DFT-calculated energies directly acquired from MP database.](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 3.50.18 PM.png>)

![Three dimensional (3-D) chemical potential diagram for the V-S-O chemical system. Energies are DFT-calculated energies directly acquired from MP database.](<../../../.gitbook/assets/image (2) (3).png>)

### Relationship to predominance diagrams

![Relationship between 3-D chemical potential diagram and predominance diagrams, which are 2-D views of the full three-dimensional chemical potential diagram surface. Figure by Matthew McDermott.](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 3.51.46 PM.png>)

## Citations

{% hint style="info" %}
### **Methodology**

Todd, P. K., McDermott, M. J., Rom, C. L., Corrao, A. A., Denney, J. J., Dwaraknath, S. S., Khalifah, P. G., Persson, K. A., & Neilson, J. R. (2021). Selectivity in Yttrium Manganese Oxide Synthesis via Local Chemical Potentials in Hyperdimensional Phase Space. Journal of the American Chemical Society, 143(37), 15185-15194. [https://doi.org/10.1021/jacs.1c06229](https://doi.org/10.1021/jacs.1c06229)
{% endhint %}



## References

\[1] Yokokawa, H. “Generalized chemical potential diagram and its applications to chemical reactions at interfaces between dissimilar materials.” JPE 20, 258 (1999). [https://doi.org/10.1361/105497199770335794](https://doi.org/10.1361/105497199770335794)

\[1] Todd, P. K., McDermott, M. J., Rom, C. L., Corrao, A. A., Denney, J. J., Dwaraknath, S. S., Khalifah, P. G., Persson, K. A., & Neilson, J. R. (2021). Selectivity in Yttrium Manganese Oxide Synthesis via Local Chemical Potentials in Hyperdimensional Phase Space. Journal of the American Chemical Society, 143(37), 15185-15194. [https://doi.org/10.1021/jacs.1c06229](https://doi.org/10.1021/jacs.1c06229)
