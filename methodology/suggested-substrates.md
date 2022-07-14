# Suggested Substrates

## Introduction

Materials synthesis techniques such as Chemical Vapor Deposition, Molecular Beam Eptixay, Sputtering, etc. are prevalent in materials research. Synthesizing materials with these techniques comes with a challenge: how does one determine which subtrate to use?

Epitaxial growth of heterogeneous interfaces requires a fundamental understanding of the substrate material, film material, cleavage planes, lattice mismatches, and resultant stresses and strains. The Materials Project (MP) stores crystallographic information for each material in its database, calculated via First Principles Density Functional Theory. Each material's crystallographic information, in particular the surface termination lattice parameters, is especially useful to find the epitaxial matches between a desired material (film) and a corresponding substrate. The Suggested Substrates tool outputs the Miller Indices of the substrate and the film (target material) termination plane, the minimal co-incident area (MCIA), and Elastic Energy.

## Calculation Details

The methodology for the Suggested Substrates tool is based off of Zurr and McGill \[1]. Suggested Substrates tool in MP relies mainly on the geometrical principles of lattice matching. Suppose there is two slabs of materials: a film and a substrate. The MP database has information of the lattice parameter for both the film slab and substrate slab. The film and the substrate slabs have a specific cleavage plane with a corresponding Miller Indices. Termination surfaces of the slabs, resultant from the cleavage, can be described by a new 2D lattice that can translate through all the sites. By interfacing the two halves, the two surfaces from the 3D slabs result in a new 2D superlattice at the interface. This 2D superlattice contains the primative translation vectors $$(\bold{a},\bold{b})$$ that describes both sides of the slab and their termination surface. Fig 1. below shows a simplified schematic of how a new lattice is created at the interface of two slabs.&#x20;

![Figure 1. Lattice matching between Si(111) and AlO(101) faces. A cell made of 21 sapphire unit cells has almost exactly the same dimensions as a cell made of 40 silicon unit cells. This can be described by a new superlattice ('supervector') at the interface. Adopted from Zur and McGill \[1\]](<../.gitbook/assets/Screen Shot 2022-07-14 at 2.52.46 PM.png>)

Two 3D slabs with given miller indices interfaced together creates a 2D lattice at their interface.&#x20;



* Geometrical Principles of Lattice matching
  * both film and substrate are 3D materials
    * when interfaced, they can create a 2D lattice at their interface
    * this 2D interface can be described as $(\mathbf{a},\mathbf{b})$, with a angle between the two of them described by parameter $\alpha$
    * There exists multiple 2D superlattices that can satisify a specific cleavge plane set for film and substrate.
  * Objective: scan across all different cleavage planes (cuts) of both sides and their 2D surfaces to determine the smallest possible values of $(\mathbf{a},\mathbf{b})$. (2D superlattice). There is also specified with a lattice mismatch in some way.
    * Look for $(\mathbf{a})$ being the shortest possible nonzero vector of the lattice
    * $(\mathbf{b})$ being the shortest possible nonzero vector of the lattice that is linearly independent of $(\mathbf{a})$
    * angle between two vectors are non-obtuse.
    * Procedure given in Fig.2
    * Find reduced primitative translation vectors, they are unique always
  * Mismatches:
    * $r\_1/r\_2 = A\_2/A\_1$, where $A\_1,A\_2$ correspond to the unit cell areas of the original lattice for lattice 1 and 2. $r\_1,r\_2$ correspond to an integer that satisfies the unit cell area being matched on the superlattice for lattice 1 and 2.
    * Since we anticipate mismatch, we can set an upper limit for the possible values of $r\_1,r\_2$ for it by setting an $A\_{max}$. to satify the equation $r\_1A\_1 \approx r\_2A\_2 \<A\_{max}$
    *



## Citations

## References

\[1] A. Zur and T. C. McGill , "Lattice match: An application to heteroepitaxy", Journal of Applied Physics 55, 378-386 (1984) https://doi.org/10.1063/1.333084
