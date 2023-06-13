# Surface Energies

## Introduction

Surface energy is a measure of the energy change associated with the breaking of intermolecular bonds in a bulk material to create a surface. In thermodynamically stable materials, the creation of a surface will always increase energy, otherwise there would be a thermodynamic driving force to create surfaces and the material would sublimate. In theory, surface energy is equal to half of the energy of cohesion (the energy needed to break all of the bonds required to form two new surfaces). However, this perfect cleaving of surfaces is rarely achieved. In reality, surfaces often rearrange and/or react with their surroundings to passivate or adsorb molecules or atoms to lower their surface energy from the theoretical cohesive energy value.

## Formalism

Surface energy is calculated using a slab model where a supercell of a crystal is oriented such that a given facet of interest is created and then exposed to vacuum by removing atoms from the supercell. If we are interested in creating a surface with the plane (_hkl_) exposed, lattice vector transformations are performed on the supercell with lattice vectors **a** and **b** parallel to the exposed plane (_hkl_) and lattice vector **c** as close to perpendicular to the exposed plane as is feasbile. This new unit cell is referred to as the oriented unit cell. The atoms in the oriented unit cell must also be shifted in the **c** direction in order to expose all possible symmetrically distinct atomic terminations. This algorithm for generating slabs is implemented in pymatgen [\[1\]](surface-energies.md#references).

The surface energy $$\gamma^{\sigma}_{hkl}$$ of facet (_hkl_) of a slab model is calculated as:

$$
\gamma^{\sigma}_{hkl} = \frac{E^{hkl,\sigma}_{slab} - E^{hkl}_{bulk} \cdot  n_{slab}}{2 \cdot A_{slab}}
$$

where $$E^{hkl,\sigma}_{slab}$$​is the total energy of the slab with termination $$\sigma$$​, $$E^{hkl}_{bulk}$$​is the per atom total energy of the bulk oriented unit cell, $$n_{slab}$$ is the total number of atoms in the slab and $$A$$ is the surface area of the slab. The bulk oriented unit cell's atomic positions as well as its volume are relaxed, whereas in the slab model, only the atomic positions are relaxed.

## DFT Parameters

All DFT calculations are performed in the Vienna Ab-initio Simulation Package (VASP) with the projector augmented wave (PAW) method. Exchange correlation effects are modeled using the Perdew-Berke-Ernzerhof (PBE) generalized gradient approximation (GGA) funcitonal. All calculations are spin polarized using a plane wave cutoff energy of 400eV. Full details can be found in [\[2\]](surface-energies.md#references).

## References

\[1]: Ong, S. P. et al. Python Materials Genomics (pymatgen): A robust, open-source python library for materials analysis. _Computational Materials Science_ **68**, 314–319 (2013)

\[2]: Tran, R., Xu, Z., Radhakrishnan, B. _et al._ Surface energies of elemental crystals. _Sci Data_ **3,** 160080 (2016)
