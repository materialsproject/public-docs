---
description: >-
  How x-ray absorption spectra are calculated on the Materials Project (MP)
  website.
---

# X-ray Absorption Spectra (XAS)

X-ray Absorption Spectra (XAS) is calculated using the code FEFF$$^{1,2,3}$$.Feff is an ab initio multiple-scattering code for calculating excitation spectra and electronic structure. It is based on a real space Green’s function approach including a screened core-hole, inelastic losses and self-energy shifts, and Debye-Waller factors. The spectra include extended x-ray absorption fine structure (EXAFS), x-ray absorption near edge structure (XANES), and then both are stiched together to give a total XAS spectra. In addition the code can treat relativistic electron energy loss spectroscopy (EELS). 

Multiple parameters are checked for convergence, including:

* _Self-consistent field (SCF)_
* _Full multiple scattering (FMS)_
* _EXCHANGE_: The EXCHANGE card specifies the exchange correlation potential model used for XANES calculation. 
* _COREHOLE_: The COREHOLE card is used for specifying how the core is treated during XANES calculation. 

For full details, please refer to publication:[ High-throughput computational X-ray absorption spectroscopy](https://www.nature.com/articles/sdata2018151)



#### References:

1. _Parameter-free calculations of x-ray spectra with FEFF9_, J.J. Rehr, J.J. Kas, F.D. Vila, M.P. Prange, K. Jorissen, Phys. Chem. Chem. Phys., 12, 5503-5513 (2010)
2. _Ab initio theory and calculations of X-ray spectra_, J.J. Rehr, J.J. Kas, M.P. Prange, A.P. Sorini, Y. Takimoto, F.D. Vila, Comptes Rendus Physique 10 (6) 548-559 (2009) 
3. _Theoretical Approaches to X-ray Absorption Fine Structure_, J. J. Rehr and R. C. Albers, Rev. Mod. Phys. 72, 621, (2000)
