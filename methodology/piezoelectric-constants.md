---
description: >-
  How piezoelectric constants are calculated for the Materials Project (MP)
  website.
---

# Piezoelectric Constants

## Introduction

Piezoelectricity is a reversible physical process that occurs in some materials whereby an electric moment is generated upon the application of a stress. This is often referred to as the direct piezoelectric effect. Conversely, the indirect piezoelectric effect refers to the case when a strain is generated in a material upon the application of an electric field. The mathematical description of piezoelectricity relates the strain (or stress) to the electric field via a third order tensor. This tensor describes the response of any piezoelectric bulk material, when subjected to an electric field or a mechanical load.

The piezoelectric constants from the Materials Project (MP) are calculated from first principles Density Functional Perturbation Theory (DFPT) [\[1\]](piezoelectric-constants.md#references) and are approximated as the superimposed effect of an electronic and ionic contribution. From the full piezoelectric tensor, several properties are derived such as the maximum longitudinal piezoelectric modulus and the corresponding crystallographic direction. Just as with the elastic constants, multiple consistency checks are performed on all the calculated piezoelectric data to ensure its reliability and accuracy.

![](../.gitbook/assets/Cubic\_pic1.png)

&#x20;_Figure 1: longitudinal piezoelectric modulus-surface for a cubic compound, showing the maximum response in the <111> family of directions._

## Formalism

In this work, we calculate the piezoelectric stress coefficients, $$\textstyle e_{ijk}^{T}$$ from DFPT, with units of $$\textstyle C/m^{2}$$. These can be defined in terms of thermodynamic derivatives as shown below [\[2\]](piezoelectric-constants.md#references).

$$
e_{ijk}^{T}= \left(\frac{\partial D_{i}}{\partial \varepsilon_{jk}}\right)_{E, T} = -\left(\frac{\partial \sigma_{jk}}{\partial E_{i}}\right)_{\varepsilon, T} \!
$$

where $$\textstyle D$$, $$\textstyle E$$, $$\textstyle \varepsilon$$, $$\textstyle \sigma$$ and $$\textstyle T$$ represent the electric displacement field, the electric field, the strain tensor, the stress tensor and the temperature, respectively.

The above relations can be written in Voigt-notation as shown below.

$$
e_{ij}^{T}= \left(\frac{\partial D_{i}}{\partial \varepsilon_{j}}\right)_{E, T} = -\left(\frac{\partial \sigma_{j}}{\partial E_{i}}\right)_{\varepsilon, T} \!
$$

We note that the most commonly used piezoelectric constants appearing in the (experimental) literature are the piezoelectric strain constants, usually denoted by $$\textstyle d_{ijk}$$. These can be readily related to the constants $$\textstyle e_{ijk}$$ if the elastic compliances $$\textstyle s_{lmjk}^{T}$$ (at constant electric field and temperature) of the materials are known: $$\textstyle d_{ijk}^{T} = e_{ilm} s_{lmjk}^{ET}$$. In particular, the piezoelectric strain constants can be expressed thermodynamically as shown below

$$
d_{kij}^{T} = \left(\frac{\partial \varepsilon_{ij}}{\partial E_{k}}\right)_{\sigma, T} = \left(\frac{\partial D_{k}}{\partial \sigma_{ij}}\right)_{E, T} \!
$$

It is well-known that the piezoelectric behavior can only occur in crystals that lack inversion symmetry. This is the direct consequence of the symmetry properties of the piezoelectric tensor, which is of order 3. Another fundamental requirement for piezoelectric behavior is that the material has a band gap. Combined, these criteria severely limit the amount of compounds in nature that have the potential to exhibit piezoelectric behavior.

For the Materials Project in particular, potential piezoelectric materials in the database are identified by i) allowing only structures with space groups 1, 3-9, 16-46, 75-82, 89-122, 143-146, 149-161, 168-174, 177-190, 195-199, 207-220 (since these space groups lack inversion symmetry), and in addition ii) the calculated DFT bandgap of the material > 0.1 eV. Compounds in the Materials Project database that satisfy these criteria are selected for a full-DFT calculation of the piezoelectric tensor and derived properties (see below).

## Derived piezoelectric properties

For elastic properties, which are based on a tensor of order 4, isotropic Voigt and Reuss averages can be derived on the bulk and shear moduli. For piezoelectric properties, this isotropic averaging-approach does not quite work due to the requirement that inversion symmetry cannot occur in piezoelectric materials. On MP, in addition to the piezoelectric tensor in Voigt-notation, we report the maximum longitudinal piezoelectric modulus of the compound and the corresponding crystallographic direction in which this occurs. One can think of these quantities as the piezoelectric counterpart of the well-known Young's modulus and the stiffest elastic direction in the context of elasticity-theory. Fig. 1 shows an example of how the longitudinal piezoelectric modulus can be represented in 3D. This is for the case of a cubic material. As can be seen clearly, the maximum modulus occurs in the <111> family of crystallographic directions. By symmetry, this is always the case for cubic piezoelectric materials. Fig. 2 shows a more complicated longitudinal piezoelectric modulus-surface for an orthorhombic compound. In that case, the relative magnitudes of the tensor components dictate in which crystallographic direction, the maximum response occurs. Finally, note that for some compounds, a piezoelectric response is only induced by shear deformation rather than tensile or compressive deformation. For these cases, the response cannot be depicted such as in Figs. 1 and 2. The representations such as in Figs. 1 and 2 and created using the open-source MTEX package [\[3,4,5\]](piezoelectric-constants.md#references).

![](../.gitbook/assets/Ortho\_1.png)

&#x20;_Figure 2: longitudinal piezoelectric modulus-surface for an orthorhombic compound._

## DFT parameters

The first-principles results presented in this work are performed using the projector augmented wave (PAW) method as implemented in the Vienna Ab Initio Simulation Package (VASP). In all calculations, we employ the Perdew, Becke and Ernzerhof (PBE) Generalized Gradient Approximation (GGA) for the exchange-correlation functional. A cut-off for the plane waves of 1000 eV is used and a uniform k-point density of approximately 2,000 per reciprocal atom (pra) is employed, which means that the number of atoms per cell multiplied by the number of k-points equals approximately 2,000. For the compounds that contain magnetic elements, a ferromagnetic state is initialized in the calculation. Similarly to our previous work, we expect to correctly converge to ferromagnetic and non-magnetic states in this way, but not to anti-ferromagnetic states. Due to the presence of strongly correlated electrons in some of the oxides, the GGA+U method is employed, with U representing the Hubbard-parameter. The values of U are chosen consistent with those employed in MP.

![](../.gitbook/assets/Piezomain.png)

&#x20;_Figure 3: A graphical representation of the piezoelectric dataset, currently containing over 900 materials. A series of concentric circles indicate constant values of the maximum longitudinal piezoelectric modulus,_ $$e_{ij,max}$$_. The compounds are broken up according to the crystal system and the different point group symmetry-classes considered in this work. See the paper_ [_A database to enable discovery and design of piezoelectric materials_](http://www.nature.com/articles/sdata201553) _for details._

## Crystal symmetry

The crystal symmetry and in particular the point group dictates the symmetry of the piezoelectric tensor, relates components of the tensor to each other and imposes that certain components equal zero. All piezoelectric tensors in the Materials Project have been symmetrized for consistency with the underlying point group of the compound. Figure 4 gives an overview of the symmetrized piezoelectric tensors in MP, broken up by the different piezoelectric point groups. Also, typical surface representations are shown. The point group that only yields piezoelectric behavior upon the application of shear is not included in the representation in Fig. 4.

![](../.gitbook/assets/Piezo\_wiki\_fig.png)

&#x20;_Figure 4: Piezoelectric tensors and symmetry classes considered in this work. Typical representations of the longitudinal piezoelectric modulus in 3D are also shown for each crystal point group. Note that depending on the components of the piezoelectric tensor, the surface representation can differ from those shown here. See the paper_ [_A database to enable discovery and design of piezoelectric materials_](http://www.nature.com/articles/sdata201553) _for details._

## Citation

To cite the piezoelectric properties within the Materials Project, please reference the following work:

* de Jong, Maarten and Chen, Wei and Geerlings, Henry and Asta, Mark and Persson, Kristin Aslaug. _A database to enable discovery and design of piezoelectric materials_, [Scientific Data 2 (2015)](http://www.nature.com/articles/sdata201553)

The paper presents the results of our piezoelectric constant-calculations for the first batch of 941 compounds. Our DFT-parameters, the workflow and comparison to experiments are described in detail. Also, the filters in the workflow used for detecting anomalies in the calculations are described in the paper.

## Authors

1. Maarten de Jong

## References

\[1]: Baroni, Giannozzi S. P. and Testa, A. Phys. Rev. Lett. 58, 1861 (1987)

\[2]: Nye, J. F. Physical properties of crystals (Clarendon press, 1985).

\[3]: Bachmann, F., Hielscher, R. & Schaeben, H. Texture analysis with MTEX-free and open source software toolbox. Solid State Phenomena 160, 63–68 (2010).

\[4]: Hielscher, R. & Schaeben, H. A novel pole figure inversion method: specification of the MTEX algorithm. Journal of Applied Crystallography 41, 1024–1037 (2008).

\[5]: Mainprice, D., Hielscher, R. & Schaeben, H. Calculating anisotropic physical properties from texture data using the MTEX open-source package. Geological Society, London, Special Publications 360, 175–192 (2011).
