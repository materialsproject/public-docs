---
description: Information on the Summary endpoint and its data.
---

# Summary

**This is the go-to endpoint for most queries.** It is a route providing a large amount of combined data for a material. This is constructed by combining subsets of data from many of the other API endpoints. The summary endpoint is very useful for performing queries for materials over a large property space. Note that every unique material within the Materials Project should have a set of summary data. See the `SummaryDoc` schema below for a full list of data fields returned, and the alternative source endpoint in which they can be found.

`MPDataEntry` objects containing `SummaryDoc` data will be returned from the`MPRester.summary.search` method.

### MPDataEntry\<SummaryDoc>

* **`builder_meta`** _(EmmetMeta)_: Builder metadata.
* **`nsites`** _(integer)_: Total number of sites in the structure. _Source endpoint: materials_
* **`elements`** _(list)_: List of elements in the material. _Source endpoint: materials_
* **`nelements`** _(integer)_: Number of elements. _Source endpoint: materials_
* **`composition`** _(dictionary)_: Full composition for the material. _Source endpoint: materials_
* **`composition_reduced`** _(dictionary)_: Simplified representation of the composition. _Source endpoint: materials_
* **`formula_pretty`** _(string)_: Cleaned representation of the formula. _Source endpoint: materials_
* **`formula_anonymous`** _(string)_: Anonymized representation of the formula. _Source endpoint: materials_
* **`chemsys`** _(string)_: dash-delimited string of elements in the material. _Source endpoint: materials_
* **`volume`** _(float)_: Total volume for this structure in Å³. _Source endpoint: materials_
* **`density`** _(float)_: Density in grams per cm³. _Source endpoint: materials_
* **`density_atomic`** _(float)_: The atomic packing density in atoms/cm³. _Source endpoint: materials_
* **`symmetry`** _(SymmetryData)_: Symmetry data for this material. _Source endpoint: materials_
* **`material_id`** _(string)_: The Materials Project ID of the material, used as a universal reference across property documents.This comes in the form: mp-\*\*\*\*\*\*. _Source endpoint: materials_
* **`deprecated`** _(boolean)_: Whether this property document is deprecated. _Source endpoint: provenance_
* **`deprecation_reasons`** _(list)_: List of deprecation tags detailing why this document isn't valid. _Source endpoint: provenance_
* **`last_updated`** _(string)_: Timestamp for the most recent calculation update for this property.
* **`origins`** _(list)_: Dictionary for tracking the provenance of properties.
* **`warnings`** _(list)_: Any warnings related to this property.
* **`structure`** _(dictionary)_: The lowest energy structure for this material. _Source endpoint: materials_
* **`task_ids`** _(list)_: List of Calculations IDs associated with this material. _Source endpoint: materials_
* **`uncorrected_energy_per_atom`** _(float)_: The total DFT energy of this material per atom in eV/atom. _Source endpoint: thermo_
* **`energy_per_atom`** _(float)_: The total corrected DFT energy of this material per atom in eV/atom. _Source endpoint: thermo_
* **`formation_energy_per_atom`** _(float)_: The formation energy per atom in eV/atom. _Source endpoint: thermo_
* **`energy_above_hull`** _(float)_: The energy above the hull in eV/Atom. _Source endpoint: thermo_
* **`is_stable`** _(boolean)_: Flag for whether this material is on the hull and therefore stable. _Source endpoint: thermo_
* **`equilibrium_reaction_energy_per_atom`** _(float)_: The reaction energy of a stable entry from the neighboring equilibrium stable materials in eV. Also known as the inverse distance to hull. _Source endpoint: thermo_
* **`decomposes_to`** _(list)_: List of decomposition data for this material. Only valid for metastable or unstable material. _Source endpoint: thermo_
* **`xas`** _(list)_: List of xas documents. _Source endpoint: xas_
* **`grain_boundaries`** _(list)_: List of grain boundary documents. _Source endpoint: grain\_boundary_
* **`band_gap`** _(float)_: Band gap energy in eV. _Source endpoint: electronic\_structure_
* **`cbm`** _(number, object)_: Conduction band minimum data. _Source endpoint: electronic\_structure_
* **`vbm`** _(number, object)_: Valence band maximum data. _Source endpoint: electronic\_structure_
* **`efermi`** _(float)_: Fermi energy in eV. _Source endpoint: electronic\_structure_
* **`is_gap_direct`** _(boolean)_: Whether the band gap is direct. _Source endpoint: electronic\_structure_
* **`is_metal`** _(boolean)_: Whether the material is a metal. _Source endpoint: electronic\_structure_
* **`es_source_calc_id`** _(string, integer)_: The source calculation ID for the electronic structure data. _Source endpoint: electronic\_structure_
* **`bandstructure`** _(BandstructureData)_: Band structure data for the material. _Source endpoint: electronic\_structure_
* **`dos`** _(DosData)_: Density of states data for the material. _Source endpoint: electronic\_structure_
* **`dos_energy_up`** _(float)_: Spin-up DOS band gap in eV. _Source endpoint: electronic\_structure_
* **`dos_energy_down`** _(float)_: Spin-down DOS band gap in eV. _Source endpoint: electronic\_structure_
* **`is_magnetic`** _(boolean)_: Whether the material is magnetic. _Source endpoint: magnetism_
* **`ordering`** _(string)_: Type of magnetic ordering. _Source endpoint: magnetism_
* **`total_magnetization`** _(float)_: Total magnetization in μB. _Source endpoint: magnetism_
* **`total_magnetization_normalized_vol`** _(float)_: Total magnetization normalized by volume in μB/Å³. _Source endpoint: magnetism_
* **`total_magnetization_normalized_formula_units`** _(float)_: Total magnetization normalized by formula unit in μB/f.u. . _Source endpoint: magnetism_
* **`num_magnetic_sites`** _(integer)_: The number of magnetic sites. _Source endpoint: magnetism_
* **`num_unique_magnetic_sites`** _(integer)_: The number of unique magnetic sites. _Source endpoint: magnetism_
* **`types_of_magnetic_species`** _(list)_: Magnetic specie elements. _Source endpoint: magnetism_
* **`k_voigt`** _(float)_: Voigt average of the bulk modulus in GPa. _Source endpoint: elasticity_
* **`k_reuss`** _(float)_: Reuss average of the bulk modulus in GPa. _Source endpoint: elasticity_
* **`k_vrh`** _(float)_: Voigt-Reuss-Hill average of the bulk modulus in GPa. _Source endpoint: elasticity_
* **`g_voigt`** _(float)_: Voigt average of the shear modulus in GPa. _Source endpoint: elasticity_
* **`g_reuss`** _(float)_: Reuss average of the shear modulus in GPa. _Source endpoint: elasticity_
* **`g_vrh`** _(float)_: Voigt-Reuss-Hill average of the shear modulus in GPa. _Source endpoint: elasticity_
* **`universal_anisotropy`** _(float)_: Elastic anisotropy. _Source endpoint: elasticity_
* **`homogeneous_poisson`** _(float)_: Poisson's ratio. _Source endpoint: elasticity_
* **`e_total`** _(float)_: Total dielectric constant. _Source endpoint: dielectric_
* **`e_ionic`** _(float)_: Ionic contribution to dielectric constant. _Source endpoint: dielectric_
* **`e_electronic`** _(float)_: Electronic contribution to dielectric constant. _Source endpoint: dielectric_
* **`n`** _(float)_: Refractive index. _Source endpoint: dielectric_
* **`e_ij_max`** _(float)_: Piezoelectric modulus. _Source endpoint: piezoelectric_
* **`weighted_surface_energy_EV_PER_ANG2`** _(float)_: Weighted surface energy in eV/Å². _Source endpoint: surface\_properties_
* **`weighted_surface_energy`** _(float)_: Weighted surface energy in J/m². _Source endpoint: surface\_properties_
* **`weighted_work_function`** _(float)_: Weighted work function in eV. _Source endpoint: surface\_properties_
* **`surface_anisotropy`** _(float)_: Surface energy anisotropy. _Source endpoint: surface\_properties_
* **`shape_factor`** _(float)_: Shape factor. _Source endpoint: surface\_properties_
* **`has_reconstructed`** _(boolean)_: Whether the material has any reconstructed surfaces. _Source endpoint: surface\_properties_
* **`possible_species`** _(list)_: Possible charged species in this material. _Source endpoint: oxidation\_states_
* **`has_props`** _(list)_: List of properties that are available for a given material. _Source endpoint: summary_
* **`theoretical`** _(boolean)_: Whether the material is theoretical. _Source endpoint: provenance_



Alternatively, the query parameters defined below can be used with direct HTTP requests, or as input parameters to the `MPRester.summary._search` method. The same parameter information can also be found [here](https://api.materialsproject.org/docs#/Summary/search\_summary\_\_get).

{% swagger src="../../.gitbook/assets/openapi.json" path="/summary/{material_id}/" method="get" %}
[openapi.json](../../.gitbook/assets/openapi.json)
{% endswagger %}

{% swagger src="../../.gitbook/assets/openapi.json" path="/summary/" method="get" %}
[openapi.json](../../.gitbook/assets/openapi.json)
{% endswagger %}

###
