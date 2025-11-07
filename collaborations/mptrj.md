# MPtrj

The Materials Project contains a large set of relaxation data across its entire database. These relaxation "trajectories," along with single-point data, for the PBE GGA and GGA+U calculations are stored in the [tasks](https://docs.materialsproject.org/frequently-asked-questions#what-is-a-task_id-and-what-is-a-material_id-and-how-do-they-differ) collection of Materials Project.  Deng _et al._ collated the energies, forces, stresses, structures, and on-site magnetic moments (when available) from this ionic step data, and used it to train a graph neural network, CHGNet \[1], for the potential energy surface of most of the periodic table.

The collected dataset is called _MPtrj_ in the [CHGNet](https://chgnet.lbl.gov/)  \[1]. Subsets of MPtrj were used in the earlier universal machine learning interatomic potentials (MLIPs) [M3GNet](https://matgl.ai/) \[2] and later ones such as MACE-MP-0 \[3].

**\[WIP]** The original MPtrj dataset is hosted on MPContribs ([explorer](https://next-gen.materialsproject.org/contribs/projects/MPtrj_2022_9)) ([bulk download](https://s3.us-east-1.amazonaws.com/materialsproject-contribs/MPtrj_2022_9/MPtrj-2022.9_full.parquet)) with a parquet format bulk download. An example notebook for working with parquet data is also [available](https://s3.us-east-1.amazonaws.com/materialsproject-contribs/MatPES_2025_1/working_with_parquet_data.ipynb).

### References

\[1] B. Deng, P. Zhong, K.J. Jun, J. Riebesell, K. Han, C. J. Bartel, and G. Ceder. "CHGNet as a pretrained universal neural network potential for charge-informed atomistic modelling". _Nature Mach. Intell._, vol. 5, pp. 1031–1041, yr. 2023. ([DOI](https://doi.org/10.1038/s42256-023-00716-3)) ([original figshare](https://figshare.com/articles/dataset/Materials_Project_Trjectory_MPtrj_Dataset/23713842?file=41619375))

\[2] C. Chen and S.P. Ong, "A universal graph deep learning interatomic potential for the periodic table". _Nature Comput. Sci._, vol. 2, pp. 718–728, yr. 2022. ([DOI](https://doi.org/10.1038/s43588-022-00349-3))

\[3] I. Batatia _et al._, "A foundation model for atomistic materials chemistry". arXiv:2401.00096v3, yr. 2025. ([DOI](https://doi.org/10.48550/arXiv.2401.00096))
