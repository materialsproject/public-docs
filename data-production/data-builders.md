---
description: Description of builders used to produce Materials Project data.
---

# Data Builders

Builders to produce MongoDB collections associated with specific categories of materials data are implemented in the [emmet-builders](https://github.com/materialsproject/emmet/tree/main/emmet-builders) software package. These take a set of MongoDB collections as input, extract and transform data from them, and then output a new collection. The schema used by builders for the input and output collections is defined by a set of standardized document models. We use a Python library called [pydantic](https://pydantic-docs.helpmanual.io) to structure these document models, and we store our documents models within the [emmet-core](https://github.com/materialsproject/emmet/tree/main/emmet-core) software package. Additionally, these models are used to define the schema for the Materials Project API which has its server-side code implemented in [emmet-api](https://github.com/materialsproject/emmet/tree/main/emmet-api).

To browse our document models defined in emmet, see [here](https://github.com/materialsproject/emmet/tree/main/emmet-core/emmet/core). For example, see the `ThermoDoc` defined [here](https://github.com/materialsproject/emmet/blob/main/emmet-core/emmet/core/thermo.py) as an example document model that powers both the [`ThermoBuilder`](https://github.com/materialsproject/emmet/blob/main/emmet-builders/emmet/builders/vasp/thermo.py) and the Materials Project's [thermo API endpoint](https://api.materialsproject.org/docs#/Thermo/get\_by\_key\_thermo\_\_material\_id\_\_\_get).

The figure below illustrates the entire Materials Project build pipeline including builders and all input/output collections:

![](../.gitbook/assets/mp\_build\_pipeline.svg)

For information on how to run any of the emmet builders, see the [Running Builders](https://materialsproject.github.io/maggma/getting\_started/running\_builders/) section of the maggma software package which defines a lot of the core builder related code and CLI.
