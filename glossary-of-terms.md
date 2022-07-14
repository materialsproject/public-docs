---
description: >-
  Terms used by the Materials Project, ordered alphabetically. Some terms are
  scientific terms while other terms refer to tools used in Materials Project
  infrastructure.
---

# Glossary of Terms

**Builder.** A builder is a little script written in the Python programming language that helps create new database collection(s) from input database collection(s). It's typically used to allow common analysis tasks to be repeated automatically, for example the calculation of "energies above hull" when new calculations are added to the database. Builders are an essential step in the Materials Project database release process and are formalized with the `emmet` code.

**Chemical system.** On Materials Project, a chemical system is a set of materials whose members all contain the same elements. It is usually noted with as dash-delimited list of elements. For example, the "Ga-In-N" chemical system would contain all materials containing Ga, In or N or combinations of these elements (Ga, In, N2, GaN, InGaN, etc.).

**Correction scheme.** The Materials Project performs calculations using a simulation technique with known systematic errors. A correction scheme is employed to adjust energies based on the elements present in a material to address these systematic errors. Only elements for which sufficient experimental data is available can be corrected.

**Energy above hull.** A measure of a material's thermodynamic stability. This value refers to a mathematical construction that can be calculated from a set of formation energies and compositions known as a convex hull, and often referred to here as a "phase diagram." However, unlike most phase diagrams, convex hulls are usually given without a temperature axis since the simulation technique used (DFT) gives predictions at zero temperature. A material which lies "on the convex hull" is predicted to be thermodynamically stable, while off the hull is predicted to be metastable or unstable. Values above 200 meV/atom are considered very large and suggest an unstable material that might not be synthesizable, however this ceiling differs significantly by chemistry. Energies above hull are given as a guide and subject to both limits of calculation precision (several meV) and also of calculation accuracy due to limitations of the simulation technique used, where errors can be significant in certain chemistries.

**Mixing scheme.** The Materials Project uses two slightly different simulation techniques depending on the elements present in a material. These are GGA (Generalized Gradient Approximation) and GGA+U, where the +U (Hubbard correction) is a correction applied to address systematic deficiencies in GGA when simulating elements with highly localized electrons such as d-orbitals or f-orbitals. Energies from these respective techniques are not directly comparable with each other, so a mixing scheme is employed such that elements can be compared. Details of the mixing scheme can be found in [this paper](https://doi.org/10.1103/PhysRevB.84.045115).
