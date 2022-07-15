---
description: Introduction to MP's contribution platform MPContribs
---

# MPContribs

**MPContribs** provides a platform and advanced programming interface (API) to contribute computational as well as experimental data to Materials Project. Data on MPContribs is collectively maintained as annotations to existing MP materials (or formulas and chemical systems), and automatically exposed to over 200,000 MP users. The platform serves as the backbone for data and apps contributed to MP while leaving full ownership and control over the data with contributors. Contributed data is automatically shown on MP's [materials details pages](https://materialsproject.org/materials/mp-22987/#contributed\_data) or its disambiguation pages for formulas and chemical systems. A dedicated landing page is provided for each MPContribs project which can be used to reference the dataset in journal publications through Digital Object Identifiers (DOIs) provided by MP in collaboration with the DOE Office of Scientific and Technological Information ([OSTI](https://www.osti.gov/)). The MPContribs [python client](https://pypi.org/project/mpcontribs-client/) can be used to programmatically retrieve, upload and modify contributed data.

See below for a list of [current MPContribs deployments](mpcontribs.md#deployments) and an overview of its [concepts](mpcontribs.md#undefined). Continue with the following sections in MP's documentation to learn more:&#x20;

* [Explore Contributed Data](apps/explore-contributed-data.md)
* [Download Contributed Data](downloading-data/query-and-download-contributed-data.md)
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

{% hint style="danger" %}
Under Construction
{% endhint %}

Each MPContribs deployment is organized into **projects**. The MP account creating the project becomes its owner. An owner can ask for the MP accounts of their collaborators to be given access to their project. A collaborator assumes the same level of permissions within a project as the owner.

A project contains a list of **contributions** to existing MP materials (or alternatively to formulas and chemical systems). It's in the owner's purview to decide what exactly constitutes a project. Often this will simply be an umbrella for a dataset containing contributions to MP materials that are comparable in their scientific context and thus are consistent in their data schema.

Any MP account can create (or be an owner of) a maximum of 3 projects at any time. Project owners can immediately start adding up to 10 contributions to their project without approval from MP. To add more contributions, project owners or their collaborators can [reach out to MPContribs administrators](mailto:contribs@materialproject.org) to obtain approval.

By default, projects are set to private, i.e. only visible to owners and their collaborators. Each individual contribution in a project is set to public by default and thus automatically released to the public when the project is published. Since the public/private flag can be controlled for each contribution individually, some contributions in a project can be kept private even if the project is public. The public/private state of a project and its contributions can be changed/reverted at any time.

A single contribution constitutes a small blob of data assigned and linked to the according MP material through identifiers such as MP's [materials IDs](frequently-asked-questions.md#what-is-a-task\_id-and-what-is-a-material\_id-and-how-do-they-differ), formulas or chemical systems. In addition to these identifiers, each individual contribution can contain the following four components:

* A **data** component containing hierarchically organized key-value data (think nested dictionaries). In its flattened format, this component can contain a maximum of 50 keys/fields each of which becomes a **column** in the overview table on the [project landing pages](apps/explore-contributed-data.md). Nested fields in the data dictionary are organized as nested columns on the landing page table. Any data types included in the data component become queryable, filterable and sortable using a wide variety of operators. See the [API documentation](mpcontribs.md#deployments) for any MPContribs deployment for a generic list of available filters.&#x20;



see the section for [contributing data](uploading-data/what-is-mpcontribs.md) for more information.
