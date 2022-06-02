---
description: Additional notes relevant to users of the legacy Materials Project API.
---

# Legacy API Users

Legacy API users will be used to querying data largely through the top level `query` method of the old `MPRester`. Additionally, MongoDB-like query syntax (e.g. `{"elements": {"$in": ["Si", "O"]}}`) was required as input, with parameters defined in the [mapidoc GitHub repository](https://github.com/materialsproject/mapidoc). **This is no longer available in the new API and `MPRester` client.**&#x20;

The major API changes are summarized below:

* The `MPRester` client can now be found in the `mp-api` Python package.
* The `query` method is replaced by the `search` method of the `summary` endpoint (i.e. `MPRester.summary.search`).
* Query parameters are defined in the client as inputs to the `search` function.

It should be noted that most of the other top level convenience function in the `MPRester` are still available (e.g. `get_bandstructure_by_material_id`, `get_entries_in_chemsys`).

**See the** [**Getting Started**](getting-started.md) **section for details on how to start using the new `MPRester` client.**&#x20;
