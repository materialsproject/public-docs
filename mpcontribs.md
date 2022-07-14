---
description: Introduction to MP's contribution platform MPContribs
---

# MPContribs

**MPContribs** provides a platform and advanced programming interface (API) to contribute computational as well as experimental data to Materials Project. Data on MPContribs is collectively maintained as annotations to existing MP materials (or formulas and chemical systems), and automatically exposed to over 200,000 MP users. The platform serves as the backbone for data and apps contributed to MP while leaving full ownership and control over the data with contributors. Contributed data is automatically shown on MP's [materials details pages](https://materialsproject.org/materials/mp-22987/#contributed\_data) or its disambiguation pages for formulas and chemical systems. A dedicated landing page is provided for each MPContribs project which can be used to reference the dataset in journal publications through Digital Object Identifiers (DOIs) provided by MP in collaboration with the DOE Office of Scientific and Technological Information ([OSTI](https://www.osti.gov/)). The MPContribs [python client](https://pypi.org/project/mpcontribs-client/) can be used to programmatically retrieve, upload and modify contributed data.

Continue with the following sections in MP's documentation to learn more:&#x20;

* [Explore Contributed Data](apps/explore-contributed-data.md)
* [Download Contributed Data](downloading-data/download-contributed-data.md)
* [Contribute your own Data](uploading-data/what-is-mpcontribs.md)

## Deployments

The table below lists the various MPContribs portals currently available. When you explore data or consider contributing your data to MP, please pick the portal that best aligns with it.

| Portal       | URLs                                                                                                                                                               | Description                                                                                                                                                       |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MPContribs   | <ul><li><a href="https://contribs.materialsproject.org">portal</a></li><li><a href="https://contribs-api.materialsproject.org">API</a></li></ul>                   | Datasets that don't fall under the purview of the other portals                                                                                                   |
| MPContribsML | <ul><li><a href="https://ml.materialsproject.org">portal</a></li><li><a href="https://ml-api.materialsproject.org">API</a></li></ul>                               | Benchmark datasets for Machine Learning                                                                                                                           |
| MPContribsLS | <ul><li><a href="https://lightsources.materialsproject.org">portal</a></li><li><a href="https://lightsources-api.materialsproject.org">API</a></li></ul>           | Datasets from beam lines at X-Ray light sources (NSLS-II, ALS, etc.)                                                                                              |
| MPContribsWS | <ul><li><a href="https://workshop-contribs.materialsproject.org">portal</a></li><li><a href="https://workshop-contribs-api.materialsproject.org">API</a></li></ul> | [Refractive index](https://refractiveindex.info/) data uploaded during [MP's workshop](https://workshop.materialsproject.org/lessons/07\_mpcontribs/contribute/). |

## Concepts

* projects
* contributions
* contribution components: data, structures, tables, attachments
