---
description: Comparison between the old (legacy) and new Materials Project (MP) APIs.
---

# Differences between new and legacy API

A summary of differences between the new and legacy API can be found in the table below. For more detailed information on differences regarding the Python API client.

|                                                      | New API                                                                                    | Legacy API                                                                                |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| **Currently recommended for**                        | Early adopters                                                                             | Everyone else                                                                             |
| **Base URL**                                         | api.materialsproject.org                                                                   | materialsproject.org/rest/v2                                                              |
| **Documentation**                                    | [api.materialsproject.org/docs](https://api.materialsproject.org/docs)                     | [mapidoc](https://github.com/materialsproject/mapidoc)                                    |
| **Specification**                                    | [OpenAPI-compliant specification available](https://api.materialsproject.org/openapi.json) | None available                                                                            |
| **Support**                                          | Our new API will be supported for the forseeable future once released                      | Will be available for at least one year after new API is finalized                        |
| **Data Updates**                                     | Will receive new data updates included latest and most accurate data                       | Will be frozen at database release v2021.03.13                                            |
| **API Key**                                          | [Available here ](https://next-gen.materialsproject.org/api#api-key)                       | Available at [legacy.materialsproject.org/open](https://legacy.materialsproject.org/open) |
| **Python client installation**                       | `pip install mp-api` (may be available in _pymatgen_ at a later date)                      | `pip install pymatgen`                                                                    |
| **Python client import code**                        | `from mp_api.client import MPRester`                                                       | `from pymatgen.ext.matproj import MPRester`                                               |
| **MPContribs integration for user contributed data** | Yes                                                                                        | No                                                                                        |
