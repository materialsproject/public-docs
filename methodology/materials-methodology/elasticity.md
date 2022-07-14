# Elastic Constants

## Introduction

Elasticity describes a material's ability to resist deformations (i.e. size and shape) when subjected to external forces. This can be thought about in two, complementary ways:

* &#x20;how much force is required to deform (stretch or compress) a material by a certain amount;
* &#x20;how much a material will deform (stretch or compress) when a certain amount of external forces is applied to that material.&#x20;

Elasticity is considered a reversible process. When the stress is removed, the material returns to its original size and shape. This is only true up to a point: if a material is stressed or strained too much, then it will be permanently changed.&#x20;

These relationships are quantified with **elastic constants** like the **elasticity tensor **_****_ and its inverse quantity, the **compliance tensor** as part of the theory of **linear elasticity.** These tensors can be used to calculate numbers such as the **bulk modulus, shear modulus or Poisson's ratio,** which are especially useful to describe simpler (isotropic) materials.

It is beyond the scope of this documentation to explain this theory, but if this concept is new to readers, a good place to start is to learn about [Hooke's Law](https://en.wikipedia.org/wiki/Hooke's\_law). Readers with mathematical backgrounds are referred to ["Physical properties of crystals: their representation by tensors and matrices" by J.F. Nye](https://books.google.com/books?id=ugwql-uVB44C).

The Materials Project predicts elastic tensors for over ten thousand materials. These are available via the Materials Project website and for direct download via the Materials Project API.

## Methodology

The elastic constants from the Materials Project (MP) are calculated from first principles Density Functional Theory (DFT). The process is started by performing an accurate structural relaxation for each structure, to a state of approximately zero stress. Subsequently, perturbations are applied to the lattice vectors and the resulting stress tensor is calculated from DFT, while allowing for relaxation of the ionic degrees of freedom. Finally, constitutive relations from linear elasticity, relating stress and strain, are employed to fit the full 6x6 elastic tensor. From this, aggregate properties such as Voigt and Reuss bounds on the bulk and shear moduli are derived. Multiple consistency checks are performed on all the calculated data to ensure its reliability and accuracy.

### Formalism

The relaxed lattice vectors $$\{\mathbf{a}_1, \mathbf{a}_2, \mathbf{a}_3\}$$ __ are taken and these are deformed according to each of the 6 deformation gradients $$\mathbf{F}$$ as shown in the equations below. These 6 deformation gradients are applied one-by-one to the relaxed structure so that only one independent deformation is considered each time. For each of the 6 deformation modes, 4 different default magnitudes of deformation are applied: $$\delta_{1} \in \{-0.01, -0.005, +0.005, +0.01\}$$ (pertaining to non shear-modes) and $$\delta_2 \in \{ -0.06, -0.03, +0.03, +0.06 \}$$ (pertaining to shear-modes). __ This leads to a total of 24 deformed structures, for which the stress tensor, $$\mathbf{S}$$, is calculated, allowing for relaxation of the ionic degrees of freedom. Note that in this work, conventional unit cells, obtained using `pymatgen.symmetry.SpacegroupAnalyzer.get_conventional_standard_structure` are employed for all elastic constant calculations. In our experience, these cells typically yield more accurate and better converged elastic constants than primitive cells, at the cost of more computational time. We suspect this has to do with the fact that unit cells often exhibit higher symmetries and simpler Brillouin zones than primitive cells (an example is face centered cubic cells).

$$
\boldsymbol{F}=
\left[{\begin{matrix}
1 + \delta _{1} & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{matrix}}\right]
$$

$$
\boldsymbol{F}=
\left[{\begin{matrix}
1 & 0 & 0 \\
0 & 1 + \delta _{1} & 0 \\
0 & 0 & 1 \\
\end{matrix}}\right]
$$

$$
\boldsymbol{F}=
\left[{\begin{matrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 + \delta _{1} \\
\end{matrix}}\right]
$$

$$
\boldsymbol{F}=
\left[{\begin{matrix}
1 & \delta _{2} & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{matrix}}\right]
$$

$$
\boldsymbol{F}=
\left[{\begin{matrix}
1 & 0 & \delta _{2} \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{matrix}}\right]
$$

$$
\boldsymbol{F}=
\left[{\begin{matrix}
1 & 0 & 0 \\
0 & 1 & \delta _{2} \\
0 & 0 & 1 \\
\end{matrix}}\right]
$$

$$
\mathbf{E}=\frac{1}{2}(\mathbf{F^T} \mathbf{F} - \mathbf{I})
$$

We employ the Green-Lagrange strain tensor, $$\mathbf{E}$$, in this work, defined above. The 6x6 elastic tensor is calculated from the equation below, with $$\mathbf{E}$$ again the Green-Lagrange strain tensor and $$\mathbf{S}$$ the calculated stress tensor. The components are calculated from a linear least-squares-fit if stress versus strain. We make use of the following Voigt-notation in this work: $$11 \mapsto 1, 22 \mapsto 2, 33 \mapsto 3, 23 \mapsto 4, 13 \mapsto 5, 12 \mapsto 6$$.

$$
\left[
\begin{matrix}
S_{11} \\
S_{22} \\
S_{33} \\
S_{23} \\
S_{13} \\
S_{12}
\end{matrix}
\right]
=
\left[
\begin{matrix}
C_{11} & C_{12} & C_{13} & C_{14} & C_{15} & C_{16} \\
C_{12} & C_{22} & C_{23} & C_{24} & C_{25} & C_{26} \\
C_{13} & C_{23} & C_{33} & C_{34} & C_{35} & C_{36} \\
C_{14} & C_{24} & C_{34} & C_{44} & C_{45} & C_{46} \\
C_{15} & C_{25} & C_{35} & C_{45} & C_{55} & C_{56} \\
C_{16} & C_{26} & C_{36} & C_{46} & C_{56} & C_{66} \\
\end{matrix}
\right]
\left[
\begin{matrix}
E_{11} \\
E_{22} \\
E_{33} \\
2 E_{23} \\
2 E_{13} \\
2 E_{12}
\end{matrix}
\right]
$$

Different choices of lattice vectors with respect to a Cartesian coordinate system may lead to elastic tensors that look different from what might be expected. For example, for the hexagonal crystal system it is commonly stated that  $$C_{11} = C_{22}$$. However, this is true under the conditions that lattice vectors $$\mathbf{a}_1$$and __ $$\mathbf{a}_2$$are both in the basal plane, whereas $$\mathbf{a}_3$$ is orthogonal to the basal plane. Hence, the elastic tensor can only be completely specified when the lattice vectors are expressed in a given coordinate system. To avoid confusion, we present the elastic tensor in two ways. First, the elastic tensor is presented for the exact choice of lattice vectors as presented on the Materials Project webpage. This is consistent with the cif-file of the "conventional standard" structure, which can also be downloaded from the Materials Project webpage. Elastic tensors can also be expressed in a standard format according to the IEEE standard. The standardized IEEE-format specifies the precise choice of lattice vectors in a coordinate system and thereby unambiguously defines the components of the elastic tensor [\[1\]](elasticity.md#references). In most cases, the elastic tensors in the POSCAR-format and the IEEE-format are identical. When the elastic tensor in POSCAR-format and IEEE-format are not identical however, they are related by a rotation. Note that IEEE tensors can be obtained using the get\_ieee\_tensor method for any of the subclasses of `pymatgen.analysis.elasticity.tensors.TensorBase` (which include ElasticTensor and PiezoTensor).

### Derived elastic properties

From the elastic tensor defined above, a number of aggregate and derived properties is calculated. These properties are all available on the Materials Project webpage and are shown in the Table 1. As described above, the elastic tensor is presented in POSCAR-format. From the elastic tensor in POSCAR-format, the compliance tensor is calculated and reported. Also reported are Voigt, Reuss and Voigt-Reuss-Hill [\[2\]](elasticity.md#references) bounds on the bulk and shear moduli for polycrystalline materials. Finally, the elastic anisotropy index [\[3\]](elasticity.md#references) and isotropic Poisson ratio are reported.

| Property                                | Unit         | Description                                                                                                          | Equation                                                                                                                    |
| --------------------------------------- | ------------ | -------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Elastic tensor, $$C_{ij}$$              | GPa          | Tensor, describing elastic behavior, corresponding to IEEE orientation, symmetrized to crystal structure             | see main text                                                                                                               |
| Elastic tensor (original), $$C_{ij}$$   | GPa          | Tensor, describing elastic behavior, unsymmetrized, corresponding to POSCAR (conventional standard cell) orientation | see main text                                                                                                               |
| Compliance tensor, $$s_{ij}$$           | GPa$$^{-1}$$ | Tensor, describing elastic behavior                                                                                  | $$s_{ij} = C_{ij}^{-1}$$                                                                                                    |
| Bulk modulus Voigt average, $$K_V$$     | GPa          | Upper bound on $$K$$ for polycrystalline material                                                                    | $$9K_{V}=\left(C_{11}+C_{22}+C_{33}\right) + 2\left(C_{12}+C_{23}+C_{31}\right)$$                                           |
| Bulk modulus Reuss average, $$K_R$$     | GPa          | Lower bound on $$K$$ for polycrystalline material                                                                    | $$1 / K_{R} = \left(s{11}+s{22}+s{33}\right) + 2\left(s{12}+s{23}+s_{31}\right)$$                                           |
| Shear modulus Voigt average, $G\_{V}$   | GPa          | Upper bound on $$G$$ for polycrystalline material                                                                    |  $$15G_{V} = \left(C{11}+C{22}+C{33}\right)-\left(C_{12}+C_{23}+C_{31}\right) + 3\left(C_{44}+C_{55}+C_{66}\right)$$        |
| Shear modulus Reuss average, $$G_R$$    | GPa          | Lower bound on $$G$$ for polycrystalline material                                                                    | $$15 / G_{R} = 4\left(s_{11}+s_{22}+s_{33}\right)-4\left(s_{12}+s_{23}+s_{31}\right) + 3\left(s_{44}+s_{55}+s_{66}\right)$$ |
| Bulk modulus VRH average, $$K_{VRH}$$   | GPa          | Average of $$K_R$$ __ and __ $$K_V$$__                                                                               | $$2 K_{VRH} = \left(K_{V} + K_{R} \right)$$                                                                                 |
| Shear modulus VRH average, $$G_{VRH}$$  | GPa          | Average of $$G_R$$ and $$G_V$$                                                                                       | $$2 G_{VRH} = \left(G_{V} + G_{R} \right)$$                                                                                 |
| Universal elastic anisotropy, $$A^{U}$$ | -            | Description of elastic anisotropy                                                                                    | $$A^{U} = 5 \left(G_{V}/G_{R}\right) + \left(K_{V}/K_{R}\right) -6 \geq 0$$                                                 |
| Isotropic Poisson ratio, $$\mu$$        | -            | Number, describing lateral response to loading                                                                       | $$\mu = \left(3K_{VRH} - 2G_{VRH}\right)$ / $\left(6K_{VRH} + 2G_{VRH}\right)$$                                             |

### DFT parameters

To obtain accurate elastic constants from DFT, a well-converged stress tensor is required. This typically means that more precise DFT-parameters have to be employed, compared to for example a simple total energy-calculation. Careful convergence testing and comparison to experimental results has led to a set of DFT-parameters that yield elastic constants, converged to within approximately 5% for over 95% of the systems. In choosing DFT-parameters for the calculations, we distinguish between metals and metallic compounds (metallics) on one hand and semiconductors and insulators (non-metallics) on the other hand. The most relevant DFT-parameters used in our HT-calculations are shown in Table 2. K-point density is expressed in per-reciprocal-atom (pra). The first-principles results presented in this work are performed using the projector augmented wave (PAW) method [\[3,4\]](elasticity.md#references) as implemented in the Vienna Ab Initio Simulation Package (VASP) [\[5,6,7\]](elasticity.md#references) . In all calculations, we employ the Perdew, Becke and Ernzerhof (PBE) [\[8\]](elasticity.md#references) Generalized Gradient Approximation (GGA) for the exchange-correlation functional. As described in the literature, several filters are used to detect cases where the elastic tensor might not have been converged properly. For those cases, the calculation is repeated but now with more stringent DFT-convergence parameters. Hence, the numerical values in Table 2 are representative for our calculations, but in some cases more strict parameters have been used. The calculation details for each compound can be found on the Materials Project webpage.

|                                | Metallics | Non-metallics |
| ------------------------------ | --------- | ------------- |
| Plane wave energy cut-off (eV) | 700       | 700           |
| Density of k-points (pra)      | 7,000     | 1,000         |
| Pseudo potential               | GGA-PBE   | GGA-PBE       |

&#x20;&#x20;

![Visualization of the current elastic-property database, consisting of over 1,100 metals and inorganic compounds. This map shows the shear and bulk moduli, together with isotropic Poisson ratio and volume-per-atom. See the paper \[Charting the complete elastic properties of inorganic crystalline compounds\](http://www.nature.com/articles/sdata20159) for details.](../../.gitbook/assets/Data\_figure\_11\_22.png)

### Symmetrization

Tensor symmetrization and IEEE conversion procedures are implemented in [pymatgen](http://pymatgen.org/\_modules/pymatgen/core/tensors.html). Symmetrization occurs by finding all of the symmetry operations that correspond to a particular crystal symmetry, and taking the average over all transformed tensors with respect to these operations. If there are $$y$$ symmetry operations are denoted $$Q_{ij}^{(x)}$$ then:

$$
C{mnop}^{(sym)} = \sum{x=1}^y Q{im}^{(x)} Q{jn}^{(x)} Q{ko}^{(x)} Q{lp}^{(x)} C_{ijkl}
$$

## How to Cite

If you use any elastic constants predicted by the Materials Project in your work, the corresponding methods paper(s) should be cited. See the [How to Cite](https://next-gen.materialsproject.org/about/cite) page for more.

## Thanks

Thanks to Maarten de Jong for the initial version of this page.

## References

1. IEEE standard on piezoelectricity. ANSI/IEEE Std 176-1987 0-1 (1988).
2. Hill, R. The elastic behaviour of a crystalline aggregate. Proceedings of the Physical Society. Section A 65, 349 (1952).
3. Ranganathan, S. I. & Ostoja-Starzewski, M. Universal elastic anisotropy index. Physical Review Letters 101, 055504 (2008).
4. Blochl, P. E. Projector augmented-wave method. Phys. Rev. B 50, 17953{17979 (1994).
5. Kresse, G. & Joubert, D. From ultrasoft pseudopotentials to the projector augmented-wave method. Phys. Rev. B59, 1758{1775 (1999).&#x20;
6. Kresse, G. & Hafner, J. Ab initio molecular dynamics for liquid metals. Phys. Rev. B 47, 558{561 (1993).
7. Kresse, G. & Furthmuller, J. Efficffient iterative schemes for ab initio total-energy calculations using a plane-wave basis set. Phys. Rev. B 54, 11169{11186 (1996).
8. Perdew, J. P., Burke, K. & Ernzerhof, M. Generalized gradient approximation made simple. Physical Review Letters 77, 3865 (1996).

