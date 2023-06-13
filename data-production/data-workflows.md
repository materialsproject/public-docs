---
description: Description of workflows used to calculate core Materials Project data.
---

# Data Workflows

Materials Project data is produced using automated high-throughput workflows implemented within the open-source [atomate](https://atomate.org/) software package. A list of the workflows used to produce the core set of materials data include:

* **Static (wf\_static)**
* **Structure Optimization (wf\_structure\_optimization)**
* **Band Structure and Density of States (wf\_bandstructure)**
* **Bulk Modulus and Equations of State (wf\_bulk\_modulus)**
* **Dielectric Constants (wf\_dielectric\_constant)**
* **Elastic Constants (wf\_elastic\_constant)**
* **Piezoelectric Constants (wf\_piezoelectric\_constant)**
* **r2SCAN Optimization (wf\_r2scan\_opt)**
* **SCAN Optimization (wf\_scan\_opt)**
* **X-ray Absorption Spectra (wf\_xas, wf\_exafs\_paths, wf\_eels)**
* **Surface Energies (wf\_slab)**

The DFT calculations run by these workflows use standardized input sets [defined within pymatgen](https://pymatgen.org/pymatgen.io.vasp.sets.html). The goal of these workflows is to produce calculation (task) data which is parsed from the raw output files using a "drone" (e.g. [atomate's vasp drone](https://atomate.org/atomate.vasp.html?highlight=drone#module-atomate.vasp.drones)) and stored within a MongoDB collection. From this, other database collections are "built" to house specific data (i.e. thermo, dielectric, xas, etc...). For more information on builders and building data, see the [Data Builders](data-builders.md) section.
