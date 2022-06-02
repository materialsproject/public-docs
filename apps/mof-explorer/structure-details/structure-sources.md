---
description: Where did each of the initial structures come from?
---

# Structure Sources

Each material in the QMOF Database, and thereby the MOF Explorer, was taken from an existing dataset of MOF structures. Some of these datasets are dedicated to experimentally synthesized MOF structures, whereas others are hypothetical MOF structures (i.e. computationally constructed). Below, we outline the various datasets of MOF structures used in constructing the QMOF Database.

#### Cambridge Structural Database – MOF Subset

The [Cambridge Structural Database](https://www.ccdc.cam.ac.uk/structures) (CSD) contains experimentally derived crystal structures for over a million materials. Of the crystal structures published on the CSD, approximately 100,000 are included in what is referred to as the [CSD MOF Subset](https://doi.org/10.1021/acs.chemmater.7b00441). It should be noted that the definition of a MOF in the CSD MOF Subset is more inclusive than many other databases and includes non-porous materials that are arguably best described as coordination polymers, in addition to more conventional MOF structures.

In the QMOF Database, structures were taken directly from the CSD MOF Subset with free (i.e. unbound) solvent removed from the pores. [ConQuest](https://doi.org/10.1107/S0108768102003324) was used to download the structures, and we excluded materials that were flagged as having charge-balancing ions, any errors in the crystal structure, or disorder in the framework. Additionally, we excluded any structures that lacked carbon or hydrogen atoms, had atoms with close interatomic distances, had lone (i.e. unbonded) atoms, or had terminal oxo ligands on metals where such ligands are typically OH groups or water. Several scripts to carry out these fidelity checks can be found [here](https://github.com/arosen93/QMOF/blob/main/database\_tools).

#### CoRE MOF Database

The [Computation-Ready, Experimental (CoRE) MOF Database](https://pubs.acs.org/doi/abs/10.1021/acs.jced.9b00835) contains experimentally derived crystal structures for \~14,000 porous, three-dimensional MOFs. The materials in the CoRE MOF Database were derived from the CSD but are not directly associated with the CSD MOF Subset, although many of the CoRE MOFs can be found in the CSD MOF Subset as well. Unlike the CSD MOF Subset, which provides as-reported crystal structures, a suite of automated and manual structural corrections were carried out during the construction of the CoRE MOF Database. As with any automated approach, not all of these structural corrections are perfect in their execution and can result in materials with misplaced atoms, under- and over-bonded atoms, charge imbalances, and similar structural fidelity issues that can be determinetal for DFT.

In the QMOF Database, we considered CoRE MOFs that were included in curated lists provided by [Chan and Manz](https://doi.org/10.1039/D0RA02498H) and [Kancharalapalli and coworkers](https://doi.org/10.1021/acs.jctc.0c01229) to increase the likelihood of having high-fidelity CoRE MOF structures. For consistency, the free solvent-removed (FSR) subset of the CoRE MOF Database was conisdered. We emphasize that there are many MOFs present in the CoRE MOF Database that we instead adopted from the CSD MOF Subset directly. As such, if a user is specifically interested in which MOFs in the QMOF Database are also present in the CoRE MOF Database, one should compare the CSD reference codes and/or MOFids for the materials in these two datasets.

#### Pyrene MOFs

Several experimentally characterized, pyrene-containing MOFs were taken from the work of [Kinik et al.](https://pubs.rsc.org/en/content/articlehtml/2021/cs/d0cs00424c) using the structures that were uploaded to the [Materials Cloud](https://doi.org/10.24435/materialscloud:z5-ct). No further modifications were made to these structures.

#### ToBaCCo

The [Topology-Based Crystal Constructor (ToBaCCo) code](https://github.com/tobacco-mofs/tobacco\_3.0) can generate hypothetical MOFs from known inorganic and organic building blocks (and topologies). Here, the "ToBaCCo" dataset of MOFs specifically refers to those found in the [original ToBaCCo paper](https://doi.org/10.1021/acs.cgd.7b00848) by Colón, Gómez-Gualdrón, and Snurr. In the QMOF Database, MOFs with triangular Cu-containing nodes were selected from the ToBaCCo dataset, as found [here](https://github.com/snurr-group/tobacco\_mofs\_mc\_0\_node).

#### Anderson and Gómez-Gualdrón

The [Anderson and Gómez-Gualdrón dataset](https://aip.scitation.org/doi/full/10.1063/5.0048736) contains hypothetical MOFs constructed using ToBaCCo. In the QMOF Database, we selected Zr-containing MOFs from this dataset. We also expanded the dataset to include hypothetical Hf-containing MOfs by exchanging the Zr species for Hf.

#### Boyd & Woo

Hypothetical MOFs in the QMOF Database were also adopted from the work of [Boyd et al.](https://www.nature.com/articles/s41586-019-1798-7) using the dataset of structures uploaded to the Materials Cloud [here](https://doi.org/10.24435/materialscloud:2018.0016/v3). These MOFs were construced using the [TOBASCCO code](https://github.com/peteboyd/tobascco), as described in prior work by [Boyd and Woo](https://pubs.rsc.org/en/content/articlehtml/2016/ce/c6ce00407e). As a result, we refer to these hypothetical MOFs as coming from the Boyd & Woo dataset.

In the QMOF Database, we adopted MOFs from select families in the Boyd & Woo daaset and occasionally made modifications to several of these MOFs to diversify our collection. For instance, we occasionally exchanged the metals in the inorganic node, and we constructed Al rod MOFs by exchanging the metals in the pre-existing V rod MOFs and protonating the bridging oxo ligands. We still refer to these structures as being derived from the Boyd & Woo dataset even though custom modifications have been made.

#### Genomic MOF Database

Hypothetical MOFs from the [Genomic MOF (GMOF) database](https://doi.org/10.1039/C9TA01752F) made available [on Figshare](https://figshare.com/s/ec378d7315581e48f1e4) were included in the QMOF Database. These structures were adopted as-is without further modification.

#### Mail-Order MOF-5s

Hypothetical MOF-5 analogues were obtained from [prior work](https://pubs.acs.org/doi/abs/10.1021/jp401920y) by Haranczyk and colleagues. See [here](http://nanoporousmaterials.org/databases) for the dataset.

#### Hypothetical MOF-74s

Hypothetical Mg-MOF-74 analogues were obtained from [prior work](https://pubs.rsc.org/en/content/articlehtml/2016/sc/c6sc01477a) by Haranczyk and colleagues.
