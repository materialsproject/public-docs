---
description: >-
  How coordination properties of metals (e.g. binding energies) are determined
  in MPcules
---

# Metal Coordination and Binding

The coordination of metals by nonmetallic molecules is important for many applications, such as chemical separations and electrolyte design. We therefore collect information on the binding properties of metals in molecules. These properties, especially thermodynamic quantities like binding energy, can be thought of in terms of the general reaction A-M → A + M, where M is a metal and A is some molecule. The process of calculating metal binding properties requires additional information about [molecular thermodynamics](molecular-thermodynamics.md), [bonding](bonding.md), [atomic partial charges](atomic-partial-charges.md), and [atomic partial spins](atomic-partial-spins.md).

The first step is to analyze the coordination environment around the metal. We look for any bonds involving the metal (see [bonding](bonding.md) for an explanation of how chemical bonds are identified), categorize them in terms of the coordinating atom (e.g. O, F, N, or B), and then calculate statistics (e.g. the average, maximum, and minimum coordinate bond length).

From there, we need to determine the oxidation state (charge and spin) of each metal in a molecule. We do this by rounding the predicted atomic partial charge and atomic partial spin to the nearest whole number. If these values are incompatible - for instance, if a Li atom is predicted to have a charge of 1 and a spin multiplicity of 2 (net spin 1) - then we shift the charge by +1 or -1 depending on which charge is closer to the predicted partial atomic charge.

After determining the proper oxidation state of the metal (and, from this, the charge and spin multiplicity of the coordinating molecule), we search for the molecule documents in MPcules corresponding to the metal (M) and the molecule with that metal removed (A). If we can find appropriate documents, then we calculate the thermodynamics for the reaction listed above.
