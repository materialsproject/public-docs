---
description: How to download the data available on https://materialsproject.org/mofs
---

# Downloading the Data

### Downloading the QMOF Database

The recommended way of downloading much of the data underlying the QMOF Database is at the following Figshare repository: [https://doi.org/10.6084/m9.figshare.13147324](https://doi.org/10.6084/m9.figshare.13147324). The data on Figshare includes DFT-optimized geometries (in XYZ and CIF format) and several tabulated properties, such as energies, partial atomic charges (DDEC6, CM5, Bader), bond orders (DDEC6), atomic spin densities (DDEC6, Bader), magnetic moments, band gaps, and more. For reproducibility purposes, we recommend noting the version of the QMOF Database you have used. Note that a mirror of the QMOF Database made to be interoperable with the Materials Project is available on [MPContribs](https://contribs.materialsproject.org/projects/qmof), which can be queried with the [MPContribs API](https://contribs-api.materialsproject.org) if desired.

Additional files and properties beyond those hosted on Figshare (e.g. VASP inputs and outputs, density of states, charge densities) can be obtained from NOMAD and Globus, as described in more detail below. 

### Downloading the VASP Files

#### NOMAD

All VASP input and output files are made available on NOMAD at the following datasets:

1. QMOF Database - PBE: [https://dx.doi.org/10.17172/NOMAD/2021.10.10-1](https://dx.doi.org/10.17172/NOMAD/2021.10.10-1)
2. QMOF Database - HLE17: [https://dx.doi.org/10.17172/NOMAD/2021.11.17-3](https://dx.doi.org/10.17172/NOMAD/2021.11.17-3).
3. QMOF Database - HSE06\*: [https://dx.doi.org/10.17172/NOMAD/2021.11.17-2](https://dx.doi.org/10.17172/NOMAD/2021.11.17-2).
4. QMOF Database - HSE06: [https://dx.doi.org/10.17172/NOMAD/2021.11.17-1](https://dx.doi.org/10.17172/NOMAD/2021.11.17-1).

Querying NOMAD by `external_id` will allow you to search by the unique QMOF ID available on the MOF Explorer. Including a supplemental query of `datasets` will allow the user to specify one of the four datasets listed above for a specified level of theory. Links to the NOMAD files for a given material are available on each material's detail page. See the "Calculation Parameters" section of the documentation for a description of the different levels of theory.

Please note that there may be more entries on NOMAD than in the MOF Explorer. This is because structures are occasionally removed from the QMOF Database if any structural fidelity issues are identified, but entries cannot be deleted from NOMAD.

To download an entire NOMAD dataset, switch from the default "Entries" view to "Datasets".

#### Globus - Charge Densities

Due to their large filesizes, charge densities are made available via a [Globus Endpoint](https://app.globus.org/file-manager?origin\_id=1083ea2f-87fa-48ea-8b34-e6bd1f5ae39a\&origin\_path=%2F). First, set up a collection end-point, which can include your local machine or a compute cluster with Globus installed. Then choose a path in the collection in which to store the files. Once this is set up, select the folders and/or files you wish to download from the QMOF collection and choose "Transfer or Sync to..." to have Globus transfer the files to your specified location.
