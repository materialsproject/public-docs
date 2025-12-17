---
description: How elastic constants are calculated on the Materials Project (MP) website.
---

# Elastic Constants

## Introduction

Elasticity describes a material's ability to resist deformations (i.e. size and shape) when subjected to external forces. This can be thought about in two, complementary ways:

*  how much force is required to deform (stretch or compress) a material by a certain amount;
*  how much a material will deform (stretch or compress) when a certain amount of external forces is applied to that material. 

Elasticity is considered a reversible process. When the force is removed, the material returns to its original size and shape. This is only true up to a point: if a material is deformed too much, then it will be permanently changed. 

For small deformations, most elastic materials exhibit linear elasticity and can be described by a linear relation between the stress and strain. These relationships are quantified with **elastic constants** like the **elasticity tensor** and its inverse quantity, the **compliance tensor**, as part of the theory of **linear elasticity.** These tensors can be used to calculate numbers such as the **bulk modulus, shear modulus, Young's modulus, and Poisson's ratio,** which are especially useful to describe the elastic behavior of isotropic materials.

It is beyond the scope of this documentation to explain this theory, but if this concept is new to you, a good place to start is to learn about [Hooke's Law](https://en.wikipedia.org/wiki/Hooke's\_law). Readers with mathematical backgrounds are referred to ["Physical properties of crystals: their representation by tensors and matrices" by J.F. Nye](https://books.google.com/books?id=ugwql-uVB44C).

The Materials Project predicts elastic constants for over ten thousand materials. These are available via the Materials Project website and for direct download via the Materials Project API.

## Methodology

### Overview 

The elastic constants from the Materials Project (MP) are calculated from first-principles Density Functional Theory (DFT). For a material, the process is started by performing an accurate structural relaxation, to a state of approximately zero stress. Subsequently, the relaxed structure is strained by changing its lattice vectors (magnitude and angle) and the resulting stress tensor is calculated from DFT, while allowing for relaxation of the ionic degrees of freedom. Finally, constitutive relations from linear elasticity, relating stress and strain, are employed to fit the full elastic tensor. From this, aggregate properties such as Voigt, Reuss, and Hill bounds on the bulk and shear moduli are derived. Multiple consistency checks are performed on all the calculated data to ensure its reliability and accuracy. For example, the $$6\times6$$ Voigt elastic matrix should be positive definite to ensure mechanical stability of a material.

### Voigt notation

Formally, the elastic tensor, $$\hat{\boldsymbol{C}}$$, is a forth-order tensor with 81 components (but only with 21 independent components):

$$
\boldsymbol{\sigma} = \boldsymbol{C}\boldsymbol{\epsilon} \quad \quad  \sigma_{ij} = C_{ijkl} \epsilon_{kl} ,
$$

where $$\boldsymbol{\sigma}$$ and $$\boldsymbol{\epsilon}$$ are the second-order stress and strain tensors, respectively, and $$i,j,k,l$$ are Cartesian indices, taking values $$x$$, $$y$$, and $$z$$. Both $$\boldsymbol{\sigma}$$ and $$\boldsymbol{\epsilon}$$ symmetric tensor, and we can represent them in [Voigt notation](https://en.wikipedia.org/wiki/Voigt\_notation) under the transformation $$xx \mapsto 1, yy \mapsto 2, zz \mapsto 3, yz \mapsto 4, xz \mapsto 5, xy \mapsto 6$$. For example, the strain transforms like $$\epsilon_1 = \epsilon_{xx}, \epsilon_2 = \epsilon_{yy}, \epsilon_3 = \epsilon_{zz}, \epsilon_4 = \epsilon_{yz},  \epsilon_5 = \epsilon_{xz},  \epsilon_6 = \epsilon_{xy}$$, and the elastic tensor transforms like $$C_{xxxx} \mapsto C_{11}, C_{xxyy} \mapsto C_{12}, .....$$Then the above linear elastic relationship can be expressed as 

$$
\left[
\begin{matrix}
\sigma_{1} \\
\sigma_{2} \\
\sigma_{3} \\
\sigma_{4} \\
\sigma_{5} \\
\sigma_{6}
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
 \epsilon_{1} \\
 \epsilon_{2} \\
 \epsilon_{3} \\
2  \epsilon_{5} \\
2  \epsilon_{5} \\
2 \epsilon_{6}
\end{matrix}
\right]
$$

The elastic tensor in Voigt notation is a $$6\times6$$ symmetric matrix, indicating that the elastic tensor has 21 independent components. 

### Formalism

With the lattice vectors$$\{\boldsymbol{a}_1, \boldsymbol{a}_2, \boldsymbol{a}_3\}$$ of the relaxed structure, a material is first deformed according to $$\hat {\boldsymbol{a}}_i = \boldsymbol{F} \boldsymbol{a}_i, ( i=1,2,3)$$. The deformation gradient $$\boldsymbol{F}$$ is obtained by solving the equation for Green-Lagrange strain $$\boldsymbol{E}$$​, namely $$\boldsymbol{\epsilon} = \boldsymbol{E} = \frac{1}{2}\left(\boldsymbol{F}^T\boldsymbol{F} - \boldsymbol{I} \right)$$, where $$\boldsymbol{I}$$ is the identify matrix and the superscript denotes matrix transpose. Then he stress tensor, $$\boldsymbol{\sigma}$$, is obtained from DFT calculation for the deformed structure with the new lattice vectors $$\{ \hat{\boldsymbol{a}}_1 ,\hat{\boldsymbol{a}}_2, \hat{\boldsymbol{a}}_3\}$$. In the DFT calculation, the lattice vectors are fixed, but the ionic degree of freedoms are allowed to relax. Six strain states (listed below) are applied one by one to the initial relaxed structure so that only one independent deformation is considered each time. For each of the six strain states, 4 different default magnitudes strains are applied: $$\delta \in \{-0.01, -0.005, +0.005, +0.01\}$$. This leads to a total of 24 deformed structures, for which the stress tensor, $$\boldsymbol{\sigma}$$, is calculated. The obtained set of 24 stresses and strains are then used in a linear fitting to compute the elastic tensor. Note that conventional unit cells, obtained using pymatgen `SpacegroupAnalyzer`, are employed for all elastic constant calculations. In our experience, these cells typically yield more accurate and better converged elastic constants than primitive cells, at the cost of more computational time. We suspect this has to do with the fact that unit cells often exhibit higher symmetries and simpler Brillouin zones than primitive cells (an example is face centered cubic cells). 

$$
\boldsymbol{\epsilon}=
\left[{\begin{matrix}
\delta \\
0\\
0 \\0 \\0 \\0 \\
\end{matrix}}\right] ,\quad 
\boldsymbol{\epsilon}=
\left[{\begin{matrix}
0 \\
\delta\\
0 \\
0 \\
0 \\
0 \\
\end{matrix}}\right] ,\quad 
\boldsymbol{\epsilon} =
\left[{\begin{matrix}
0 \\
0\\
\delta \\
0 \\
0 \\
0 \\
\end{matrix}}\right] ,\quad \boldsymbol{\epsilon} =
\left[{\begin{matrix}
0 \\
0\\
0 \\
2\delta \\
0 \\
0 \\
\end{matrix}}\right] ,\quad 
\boldsymbol{\epsilon} =
\left[{\begin{matrix}
0 \\
0\\
0\\
0\\
2\delta \\
0 \\
\end{matrix}}\right] ,\quad 
\boldsymbol{\epsilon} =
\left[{\begin{matrix}
0 \\
0\\
0\\
0\\
0\\
2\delta \\
\end{matrix}}\right].
$$

Different choices of lattice vectors with respect to a Cartesian coordinate system may lead to elastic tensors that look different from what might be expected. For example, for the hexagonal crystal system it is commonly stated that  $$C_{11} = C_{22}$$. However, this is true under the conditions that lattice vectors $$\boldsymbol{a}_1$$and $$\boldsymbol{a}_2$$are both in the basal plane, whereas $$\boldsymbol{a}_3$$ is orthogonal to the basal plane. Hence, the elastic tensor can only be completely specified when the lattice vectors are expressed in a given coordinate system. To avoid confusion, we present the elastic tensor in two ways. First, the elastic tensor is presented for the exact choice of lattice vectors as presented on the Materials Project webpage. This is consistent with the cif-file of the "conventional standard" structure, which can also be downloaded from the Materials Project webpage. Elastic tensors can also be expressed in a standard format according to the IEEE standard. The standardized IEEE-format specifies the precise choice of lattice vectors in a coordinate system and thereby unambiguously defines the components of the elastic tensor [\[1\]](elasticity.md#references). In most cases, the elastic tensors in the POSCAR-format and the IEEE-format are identical. When the elastic tensor in POSCAR-format and IEEE-format are not identical however, they are related by a rotation, which can be obtained using the `get_ieee_rotation` method `pymatgen.core.tensors.Tensor` (including the elastic tensor).

### Derived elastic properties

From the elastic tensor defined above, a number of aggregate and derived properties is calculated. These properties are all available on the Materials Project webpage and are shown in the below Table. We report Voigt, Reuss and Voigt-Reuss-Hill [\[2\]](elasticity.md#references) bounds on the bulk and shear moduli for polycrystalline materials. Finally, the elastic anisotropy index [\[3\]](elasticity.md#references) and isotropic Poisson ratio are reported.

| Property                                | Unit         | Description                                                                                                          | Equation                                                                                                                    |
| --------------------------------------- | ------------ | -------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Elastic tensor, $$C_{ij}$$              | GPa          | Tensor, describing elastic behavior, corresponding to IEEE orientation, symmetrized to crystal structure             | see main text                                                                                                               |
| Elastic tensor (original), $$C_{ij}$$   | GPa          | Tensor, describing elastic behavior, unsymmetrized, corresponding to POSCAR (conventional standard cell) orientation | see main text                                                                                                               |
| Compliance tensor, $$s_{ij}$$           | GPa$$^{-1}$$ | Tensor, describing elastic behavior                                                                                  | $$s_{ij} = C_{ij}^{-1}$$                                                                                                    |
| Bulk modulus Voigt average, $$K_V$$     | GPa          | Upper bound on $$K$$ for polycrystalline material                                                                    | $$9K_{V}=\left(C_{11}+C_{22}+C_{33}\right) + 2\left(C_{12}+C_{23}+C_{31}\right)$$                                           |
| Bulk modulus Reuss average, $$K_R$$     | GPa          | Lower bound on $$K$$ for polycrystalline material                                                                    | $$1 / K_{R} = \left(s{11}+s{22}+s{33}\right) + 2\left(s{12}+s{23}+s_{31}\right)$$                                           |
| Shear modulus Voigt average, $G\_{V}$   | GPa          | Upper bound on $$G$$ for polycrystalline material                                                                    |  $$15G_{V} = \left(C{11}+C{22}+C{33}\right)-\left(C_{12}+C_{23}+C_{31}\right) + 3\left(C_{44}+C_{55}+C_{66}\right)$$        |
| Shear modulus Reuss average, $$G_R$$    | GPa          | Lower bound on $$G$$ for polycrystalline material                                                                    | $$15 / G_{R} = 4\left(s_{11}+s_{22}+s_{33}\right)-4\left(s_{12}+s_{23}+s_{31}\right) + 3\left(s_{44}+s_{55}+s_{66}\right)$$ |
| Bulk modulus VRH average, $$K_{VRH}$$   | GPa          | Average of $$K_R$$ and $$K_V$$                                                                                       | $$2 K_{VRH} = \left(K_{V} + K_{R} \right)$$                                                                                 |
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
5. Kresse, G. & Joubert, D. From ultrasoft pseudopotentials to the projector augmented-wave method. Phys. Rev. B59, 1758{1775 (1999). 
6. Kresse, G. & Hafner, J. Ab initio molecular dynamics for liquid metals. Phys. Rev. B 47, 558{561 (1993).
7. Kresse, G. & Furthmuller, J. Efficffient iterative schemes for ab initio total-energy calculations using a plane-wave basis set. Phys. Rev. B 54, 11169{11186 (1996).
8. Perdew, J. P., Burke, K. & Ernzerhof, M. Generalized gradient approximation made simple. Physical Review Letters 77, 3865 (1996).

