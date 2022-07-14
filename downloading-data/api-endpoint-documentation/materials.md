---
description: Information on the Materials endpoint and its data.
---

# Materials

Route for "core" information associated with a given material in the Materials Project database. The unique identifier for a material is its `material_id` (e.g. `mp-149`). Core data in this context refers to the crystal structure, information associated with it such as the density and chemical formula, and the associated calculations which are identified with unique `task_id` values. It does not contain any materials properties such as the formation energy or band gap, please consult `summary` or other property-specific endpoints for this information. See the `MaterialsDoc` schema for a full list of fields returned by this route.

`MPDataEntry` objects containing `MaterialsDoc` data will be returned from the`MPRester.materials.search` method.

### MPDataEntry\<MaterialsDoc>

* **`builder_meta`** _(EmmetMeta)_: Builder metadata
* **`nsites`** _(integer)_: Total number of sites in the structure
* **`elements`** _(list)_: List of elements in the material
* **`nelements`** _(integer)_: None
* **`composition`** _(dictionary)_: Full composition for the material
* **`composition_reduced`** _(dictionary)_: Simplified representation of the composition
* **`formula_pretty`** _(string)_: Cleaned representation of the formula
* **`formula_anonymous`** _(string)_: Anonymized representation of the formula
* **`chemsys`** _(string)_: dash-delimited string of elements in the material
* **`volume`** _(float)_: Total volume for this structure in Å³
* **`density`** _(float)_: Density in grams per cm³
* **`density_atomic`** _(float)_: The atomic packing density in atoms per cm³
* **`symmetry`** _(SymmetryData)_: Symmetry data for this material
* **`material_id`** _(string)_: The ID of this material, used as a universal reference across proeprty documents.This comes in the form and MPID or int
* **`structure`** _(dictionary)_: The best structure for this material
* **`deprecated`** _(boolean)_: Whether this materials document is deprecated.
* **`deprecation_reasons`** _(list)_: List of deprecation tags detailing why this materials document isn't valid
* **`initial_structures`** _(list)_: Initial structures used in the DFT optimizations corresponding to this material
* **`task_ids`** _(list)_: List of Calculations IDs used to make this Materials Document
* **`deprecated_tasks`** _(list)_: None
* **`calc_types`** _(dictionary)_: Calculation types for all the calculations that make up this material
* **`last_updated`** _(string)_: Timestamp for when this document was last updated
* **`created_at`** _(string)_: Timestamp for when this material document was first created
* **`origins`** _(list)_: Dictionary for tracking the provenance of properties
* **`warnings`** _(list)_: Any warnings related to this material

Alternatively, the query parameters defined below can be used with direct HTTP requests, or as input parameters to the `MPRester.materials._search` method. The same parameter information can also be found [here](https://api.materialsproject.org/docs#/Materials/search\_materials\_\_get).

{% swagger src="../../.gitbook/assets/openapi.json" path="/materials/{material_id}/" method="get" %}
[openapi.json](../../.gitbook/assets/openapi.json)
{% endswagger %}

{% swagger src="../../.gitbook/assets/openapi.json" path="/materials/" method="get" %}
[openapi.json](../../.gitbook/assets/openapi.json)
{% endswagger %}

###
