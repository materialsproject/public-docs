---
description: >-
  Description of the density functional theory (DFT) functionals and level of
  theory used in MOF calculation results displayed on the Materials Project (MP)
  website.
---

# Density Functionals

In all cases, the geometries are DFT-optimized structures at the PBE-D3(BJ) level of theory, and all properties are derived from single-point (i.e. static) calculations on these PBE-D3(BJ) optimized structures. In general, most properties are presented at the PBE-D3(BJ) level of theory. However, certain properties (e.g. band gaps, partial charges) for select materials are also provided based on HLE17, HSE06\*, and HSE06 single-point calculations on the PBE-D3(BJ) optimized structures. Conventionally, these would be referred to as PBE-D3(BJ), HLE17//PBE-D3(BJ), HSE06\*//PBE-D3(BJ), and HSE06//PBE-D3(BJ), respectively. However, for brevity, we typically refer to them as PBE, HLE17, HSE06\*, and HSE06. The PBE functional is a generalized gradient approximation (GGA) functional, HLE17 is a high-local-exchange meta-GGA functional, HSE06 is a screened hybrid functional with 25% Hartree-Fock (HF) exchange, and HSE06\* is the same as HSE06 but with 10% HF exchange. For computational efficiency, the HLE17, HSE06\*, and HSE06 calculations were carried out with a _k_-point grid of 500/(number of atoms per cell).
