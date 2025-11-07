---
description: >-
  Development of robust, high fidelity datasets for training universal machine
  learning interatomic potentials
---

# r2SCAN datasets

## MatPES

The materials potential energy surface (MatPES) collaboration aims to generate low noise, high coverage small datasets of DFT-computed properties (energies, forces, stresses, magnetic moments, etc.) for training universal machine learning interatomic potentials.

The data is generated with the [`MatPESStaticSet`](https://github.com/materialsproject/pymatgen/blob/f9d9fe8e0ce09ef30cc03bcc4e9937d27afd5a6a/src/pymatgen/io/vasp/sets.py#L1655) in pymatgen, with efficient PBE and r<sup>2</sup>SCAN workflows implemented in [atomate2](https://github.com/materialsproject/atomate2/blob/457f017fd82ecfb67aec10c794600874bfbbeaf7/src/atomate2/vasp/flows/matpes.py#L25).

The full dataset can be[ downloaded from MPContribs](https://materialsproject-contribs.s3.amazonaws.com/index.html#MatPES_2025_1/) and uses the [MatPESTrainDoc schema](https://github.com/materialsproject/emmet/blob/56840ac7110096636565809cd72036fbb064392e/emmet-core/emmet/core/ml.py#L393) from `emmet-core`

## MP-ALOE

In a similar vein, Kuner _et al._ \[2] sought to expand both the size and chemistries chosen in an r<sup>2</sup>SCAN dataset, and used an active learning method to explore under-sampled regions of the potential energy surface. The resultant Materials Project active learning of off-equilibrium structures (MP-ALOE) dataset contains \~900,000 r<sup>2</sup>SCAN calculations which are compatible with MatPES.

**\[WIP]** MP-ALOE will similarly be available on MPContribs (explorer) (bulk download).

## References:

\[1] A. D. Kaplan, R. Liu, J. Qi, T. W. Ko, B. Deng, J. Riebesell, G. Ceder, K. A. Persson, and S. P. Ong, “A\
foundational potential energy surface dataset for materials,” arXiv:2503.04070, yr. 2025 ([DOI](https://doi.org/10.48550/arXiv.2503.04070)) ([MPContribs explorer](https://next-gen.materialsproject.org/contribs/projects/MatPES_2025_1)) ([website](https://matpes.ai/))

\[2] M.C. Kuner, A.D. Kaplan, K.A. Persson, M. Asta, and D.C. Chrzan, "MP-ALOE: An r<sup>2</sup>SCAN dataset for universal machine learning interatomic potentials". arXiv:2507.05559, yr. 2025. ([DOI](https://doi.org/10.48550/arXiv.2507.05559)) ([original figshare](https://doi.org/10.6084/m9.figshare.29452190.v2))
