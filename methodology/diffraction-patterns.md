---
description: How diffraction patterns are calculated on the Materials Project (MP) website.
---

# Diffraction Patterns

## Introduction

Diffraction occurs when waves (electrons, x-rays, neutrons) scattering from obstructions act as secondary sources of propagation. In the case of crystal structures, atoms in periodic lattices act as scattering sites from which constructive and destructive interference can occur. Line spectra of scattering intensity as a function incident angle can give powerful information into the planar spacing and symmetries of a crystalline material.&#x20;

### X-ray Diffraction Formalism

The calculation of x-ray diffraction patterns (XRD) in the Materials Project relies on the diffraction condition in reciprocal space[\[1\]](diffraction-patterns.md#references):&#x20;

$$
\bold{k}=\bold{k'}+\bold{g}_{hkl}
$$

where $$\bold{k}$$ is the wave vector of the incident x-ray, $$\bold{k'}$$ is the wave​vector is the scattered x-ray and $$\bold{g_{hkl}}$$is the reciprocal lattice vector of the parallel set of diffracting planes with miller indices hkl. The length of reciprocal lattice plane vector $$\bold{g_{hkl}}$$ is given by:&#x20;

$$
|\bold{g_{hkl}}| =\frac{2\sin \theta}{\lambda}
$$

where $$\lambda$$ is the wavelength of the x-ray. Therefore, the maximum diffraction plane condition which is searched is $$|\bold{g_{hkl}}| =\frac{2}{\lambda}$$. Once all of the relevant diffraction planes with reciprocal lattice vectors within this limit are selected, the diffraction condition for each of these planes can be calculated:&#x20;

$$
\sin( \theta) =  \frac{ \lambda}{2d_{hkl}}
$$

where $$d_{hkl}$$ is the is the spacing of the hkl plane. The structure factor for each of these diffraction conditions is calculated as:&#x20;

$$
F_{hkl} =  \sum \limits_{j=1}^N f_j  \exp(2 \pi i \ \mathbf{g_{hkl}}
            \cdot  \mathbf{r})
$$

where $$j$$ is the index for the $$N$$ atoms in the unit cell, $$\bold{r}$$​ is the basis vector for the atoms in the unit cell. The atomic scattering factor $$f$$​ is given by:

$$
f(s) = Z - 41.78214 \times s^2 \times \sum \limits_{i=1}^n a_i \
            \exp(-b_is^2)
$$

where $$s = \frac{\sin(\theta)}{\lambda}$$, $$a_{i}$$ and $$b_{i}$$ are parameters fitted to individual elements. ​The intensity of each diffraction condition is given by the squared modulus of the structure factor.&#x20;

Finally the Lorentz-polarization factor is applied to correct for the change in x-ray amplitude due to scattering angle and geometry of the experimental conditions:

$$
P( \theta) =  \frac{1 +  \cos^2(2 \theta)}
           { \sin^2( \theta) \cos( \theta)}
$$

### Electron Diffraction&#x20;

The Transmission Electron Microscopy (TEM) pattern for multiple Laue zones is calculated similarly to the XRD diffraction patterns and is available through the diffraction properties tab in the materials explorer. &#x20;

## References

\[1]: De Graef, Marc, and Michael E. McHenry. _Structure of materials: an introduction to crystallography, diffraction and symmetry_. Cambridge University Press, 2012.
