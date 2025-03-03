---
description: A changelog of Materials Project (MP) database releases.
---

# Database Versions

This page contains a summary of major changes for each version of the Materials Project database.

We are aware of a community need for more detailed change logs, and hope to improve our reporting for future database versions.

{% hint style="info" %}
Database versions are labelled via the date they become generally available to the public. For advanced users, the public database label is mapped to an internal pull request number, and this is accessible in the API via the `builder_meta` key.
{% endhint %}

{% hint style="info" %}
You can verify the current database version powering the website on the footer of every page. If you are using the API, there is a `get_database_version` method available.
{% endhint %}

## v2025.02.12.post

This version went live on February 28th, 2025 at about 6:30pm Pacific.

This is a patch release addressing localized data issues.

#### Elasticity Collection Updates

* Fixed an input validation error in the elasticity builder's task document processing, emmet [#767](https://github.com/materialsproject/emmet/pull/1199)
* Resolved tensor fitting failures for 2,484 compounds (approximately 28% increase in valid tensors)
* Reduced number of entries with unreasonable elastic moduli ([MatSci forum issue](https://matsci.org/t/unreasonable-elastic-moduli/61426))

## v2025.02.12

This version went live on February 12th, 2025 at about 4pm Pacific.

#### New Content

* Added 1,073 ytterbium (Yb) materials recalculated using the Yb\_3 pseudo-potential and re-relaxed with r2SCAN
* Added \~30 new hybrid inorganic/organic formate perovskites

#### Electronic Structure Data Updates

* Improved consistency of magnetic ordering assignment for structures in the electronic structure and summary collections

#### Battery Explorer/Electrode Collection Updates

* Fixed data loss issue from v2024.12.18 release, restoring missing entries in the electrodes collection as reported on [MatSci](https://matsci.org/t/what-did-the-2024-12-update-bring-to-battery-explorer/60284/1)
* Resolved upstream builder dependencies that caused the initial data loss

## v2024.12.18

This version went live on December 20th, 2024 at about 11pm Pacific.

Major updates include the addition of r2SCAN calculations and improvements to thermodynamic data handling.

**New Content:**

* Added 15,483 GNoME-originated materials calculated using r2SCAN. We remind our users that the GNoME structures are licensed BY-NC (non-commerical purposes). Explicitly accepting theBY-NC license is now required to access the GNoME dataset in the Materials/GNoME explorers and the API. A further release of an additional \~100k GNoME materials is in preparation.

**Core Changes:**

* Modified the `run_type`requirement for the definition of a valid material (`emmet-core`[622f2e3](https://github.com/materialsproject/emmet/commit/622f2e3035b9bcf66bccc653af03618a0d7d84b9)):
  * **Previously**: A material was required to have at least one `GGA(+U)`calculation
  * **Now**: MP accepts materials with only r2SCAN calculations, as going forward MP will be prioritizing r2SCAN workflows
  * This change restored 736 previously deprecated materials&#x20;

**Thermodynamic Data Updates:**

* New hierarchy for `thermo`data presentation:&#x20;
  * Affects Materials Explorer and `MPRester().summary` endpoint&#x20;
  * Resolves display issues on the Materials Explorer for 586 materials with valid thermodynamic data that were found to have failed to generate thermodynamic stability data using MP's [GGA/GGA+U/r2SCAN Mixing scheme](https://docs.materialsproject.org/methodology/materials-methodology/thermodynamic-stability/thermodynamic-stability/gga-gga+u-r2scan-mixing)
  * These values were not passed through to the `summary` endpoint as a strict `thermo_type`of `GGA_GGA+U_R2SCAN`was required
  * New preference order for `thermo_type: GGA_GGA+U_R2SCAN`> `r2SCAN`> `GGA_GGA+U`
  * The `thermo_type`for a material can be found on the material's Material Detail Page under Properties in the Thermodynamic Stability tab

## v2024.11.14

This version went live on December 12th, 2024 around noon Pacific.

* Transition in document schemas for the `tasks`  collection:
  * Part of forward-looking transition from `atomate` to `atomate2`workflow orchestration package
  * Previous: `emmet.core.vasp.task_valid.TaskDocument`
  * Current: `emmet.core.tasks.TaskDoc`
  * Accessing fields is slightly different with `TaskDoc` (ex: each `Calculation` in `calcs_reversed` is no longer accessed like a dict, but as a `Calculation`  object), but `TaskDoc` should be fully backwards compatible with operations on `TaskDocument`.
  * `TaskDoc` has some advantages over `TaskDocument`, such as dynamically updating `task_type`, `run_type`, and `calc_type`. This would avoid long-term errors such as noted below for certain NSCF calculations, or noted issues with incorrectly parsing r²SCAN meta-GGA calcs as  (PBE) GGA
* 21,144 `tasks` were incorrectly assigned a `task_type` of `NSCF Uniform`  when they were really `NSCF Line.` `NSCF Uniform` tasks are used to calculate DOSes, `NSCF Line`tasks are used to generate band structure scans. These and associated properties in `materials/summary` (band gaps, DOS, etc.) have been corrected.
* 39,374 materials were mistakenly assigned a DOS from a deprecated `NSCF Uniform` task. These have been corrected  and removed
* The current set of 2,047 GNoME-originated materials has been deprecated in preparation for a release of about 120,000 GNoME materials
* While the XAS data has not changed, be sure to update to the newest version of `pymatgen` to avoid issues parsing certain XAS tasks

## v2023.11.1

* Improved and expanded set of elasticity data. Note that there are schema changes with how it is accessed in `SummaryDoc` and `ElasticityDoc`.&#x20;
* Conversion electrode data added alongside existing insertion electrode data.
* \~10k new materials added, with \~5k deprecated. This includes a temporary deprecation of all compounds containing `Yb` while they are being re-run. This is in response to pseudopotential issues identified which were providing incorrect energies.

## v2022.10.28

This database build incorporates Materials Project’s (R2)SCAN calculations as pre-release data. The default fields returned by the website and API will remain unchanged from the previous release at the GGA(+U) level of theory, but the (R2)SCAN data is now available for advanced users. Either see the “Pre-release Data” section of a relevant material details page, generate an R2(SCAN) phase diagram with the Phase Diagram app, or access the data via the thermo API endpoint. This database release also incorporates several new perovskite materials from a collaboration with Zachary Bare, University of Colorado.

## v2021.11.10

This will be the first release with our new website and API. It does not contain any new data but is built using our new database building methods and is largely consistent with the previous database release. Some changes exist to the previous release due to improvements to detection of multi-anion systems leading to changes in the applied formation energy corrections.

{% hint style="warning" %}
Be aware, database version v2021.11.10 onwards is _only_ available on the new Materials Project website and API. The [legacy website](https://legacy.materialsproject.org) and [legacy API](downloading-data/differences-between-new-and-legacy-api.md) are frozen to the v2021.05.13 database release.
{% endhint %}

## v2021.05.13

This release updates the energy correction scheme we use to generate phase diagrams and compute formation energies. **As with any new database release, formation energies for many compounds have changed**; however in this case the change is due only to our new energy correction scheme and not to any new data. We are proud to report that the new correction scheme has reduced the overall error in formation energy in our database by 7% compared to experiment.

You can see details of each correction that has been applied by inspecting the `energy_adjustments` attribute of a `ComputedEntry` retrieved via the API. In addition, the new correction scheme is available for manual use via the `MaterialsProject2020Compatibility` class in pymatgen.

We realize that this change may be disruptive to ongoing work, and want to assure you that the historical corrections are still available in pymatgen if needed. They may be recovered by manually reprocessing `ComputedEntry` using the legacy `MaterialsProjectCompatibility` class. An example notebook demonstrating how to do this available [on matgenb 25](https://github.com/materialsvirtuallab/matgenb/blob/3dc1e275f0a9ceadc032d83d71601676530d736e/notebooks/2021-5-12-Explanation%20of%20Corrections.ipynb).

Below we summarize the most significant changes associated with the new `MaterialsProject2020Compatibility` correction scheme. For complete details and documentation, please refer to [this manuscript 32](https://chemrxiv.org/articles/preprint/A_Framework_for_Quantifying_Uncertainty_in_DFT_Energy_Corrections/14593476).

**1. Refitted corrections for legacy species**\
Corrections applied to oxygen compounds, diatomic gases, and transition metal oxides and fluorides have been refit using more up to date DFT calculations and a larger compilation of computed and experimental formation enthalpy data.

**2. Corrections for additional species**\
We have added corrections for Br, I, Se, Si, Sb, and Te, which did not previously have energy corrections. As a result, formation energies for materials containing these species will generally be lower than they were previously.

**3. Diatomic gas corrections moved to compounds**\
Previously, corrections for H, F, Cl, and N were applied to the elements. One consequence of this was that polymorphs of H2, N2, Cl2 and F2 were _alway&#x73;_&#x61;ssigned a zero energy above hull, even if some polymorphs were higher in energy. This made interpretation of these values confusing. With this release, energy corrections are applied to the material (e.g., LiH) and not the element. This also means that unstable polymorphs of diatomic gases will now have non-zero `e_above_hull`

**4. Oxidation state based corrections**\
Our build process now estimates the likely oxidation states of each species in a material, and uses this information to intelligently apply corrections to anionic species only when their estimated oxidation state is negative. For example, in the compound `MoCl3O`, estimated oxidation states for both Cl and O are negative, so both anions receive corrections.

Our algorithms are not always successful in predicting the oxidation state. When this occurs, we apply anion corrections to only the most electronegative element in the material. As a result, some ternary or higher compounds in the database may be destabilized in this release because their oxidation states could not be determined. This is the case for MoCl5O (mp-1196724) for example, which does not receive a Cl correction because O is more electronegative.

If this affects your work, you can manually assign oxidation states by populating the `oxidation_states` key of the `.data` attribute of any `ComputedEntry` and then reprocessing the data using `MaterialsProject2020Compatibility`.

**5. Uncertainty Quantification**\
We now compute the estimated uncertainty associated with the energy corrections on a material. Uncertainties reflect the measured uncertainty in the underlying experimental data that we use to determine the corrections, as well as uncertainty associated with the fitting procedure itself. This information enables new methods of assessing phase stability, as described in [this manuscript 32](https://chemrxiv.org/articles/preprint/A_Framework_for_Quantifying_Uncertainty_in_DFT_Energy_Corrections/14593476)

**A Note for API and MPRester Users**

For API users, if you are retrieving formation energies directly via the API, you will get the correct, latest formation energies from the current database release. However, if you are using `get_entries` or `get_pourbaix_entries` which apply the correction scheme on-the-fly, make sure to update to the latest version of pymatgen (v2022.0.8 or later) to get the correct values. If you are using pymatgen v2021 or earlier, this will use the old correction scheme by default when using `get_entries` and `get_pourbaix_entries`.

## v2021.03.22

This release updates some older materials with new calculations, and adjusts our rules for deprecating older calculations. It does not contain any new materials. Thanks to the new calculations many materials that were previously deprecated are now accessible again. This release is in preparation for a switch to our new compatibility scheme which will improve our predictions of formation energy.

## v2021.02.08

We had a small new database release today, this introduces new higher-quality calculations for around 30,000 materials. It also deprecates 78 materials since we currently do not have calculations for these materials that match our current quality standards; we hope to restore these 78 materials in a subsequent release. For an exact list, please see the attached file.

As a reminder, all historical calculation tasks remain available via our API and the task detail pages, and information on deprecated materials also remain available via the API. More information on our deprecation policy is in our documentation. We continue our work on better ways communicate database diffs and to more easily provide access to historical information, so stay tuned for future announcements here.

[db\_v2020\_09\_08\_to\_v2021\_02\_08\_diff.yaml](https://matsci.org/uploads/short-url/CITebuwCI46kWUbfjIWfMFf6ye.yaml) (376.9 KB)

## v2020.09.08

This releases addresses issues noticed in the previous release with formation energies and updates the energies of approximately 6k materials where this error was greatest. We are planning a further supplemental release.

We’re also looking at ways to put in place a process to be more transparent with database changes and updates to share more specifically what has changed, as well as providing means to access historical versions of the database, since we know this is a common requirement.

Note that, wherever possible, we continue to keep individual historical calculation data available via its _task\_id_ even in cases where the aggregated information (such as that presented on the materials detail page) might change.

**V2020.08.20 Released**

In this release we have added thousands of new band structure and density of states calculations, improving our overall material coverage and data quality. Additionally, we have overhauled the plotting for these quantities on the material details page. This is a first step in improving the electronic structure data within the Materials Project as part of our [new tool set 46](https://www.nature.com/articles/s41524-020-00383-7) for band structure calculations.

We are also working through an on-going issue affecting the energies of a small number of materials. In the previous release, we added a large batch of higher-quality calculations for our energetics as well as fixing numerous bugs. However, we discovered an error in our calculation parameters leading to larger energies than expected for a minority of materials and issues such as those discussed [here 27](https://matsci.org/t/inconsistent-energy-between-mp-756366-and-mp-763752-li/4648). We are currently re-running these calculations and will be fixing this data in a supplemental update in the next few weeks. We advise anybody performing large screening studies to do so with caution or wait until this supplemental update has been released.

## v2020.06

In this database release, we have added several thousand materials and many magnetic ground states, improved the quality of our energetics, and fixed many bugs. This database release is part of on-going efforts in 2020 to improve database reliability and quality, following the introduction of our deprecation process last year. There are still known issues with this release which we are working to address, please let us know if you encounter any in our forum.

## **v2019.12.05**

The issue mentioned in 2019-12-04 has now been addressed, however approximately 7% of materials saw errors in their reported energies above hull of greater than 0.05 eV/atom. Values calculated via _pymatgen_ or via the phase diagram app on the website during this time were correct, while values reported on the materials details page and via the `e_above_hull` API key were incorrect.

We encourage users who accessed convex hulls from the website between the latest database release and 2019-12-05 to re-check any values obtained from the website.

We apologize for the error, and will be incorporating additional checks into our automated testing to prevent similar errors in the future.

## **v2019.12.04**

We are aware of an on-going issue with the reported energies above hull on the materials detail pages. We will update this thread when a fix has been fully implemented and with further details.

Until this issue is fully resolved, correct energies above hull can be retrieved using _pymatgen_ as follows:

```
from pymatgen import MPRester
from pymatgen.analysis.phase_diagram import PhaseDiagram

with MPRester(YOUR_API_KEY_HERE) as mpr:
    # replace with your elements and mp-id of interest
    entries = mpr.get_entries_in_chemsys(['Li','Co', 'O'])
    entry_of_interest = mpr.get_entry_by_material_id('mp-19128')

phase_diagram = PhaseDiagram(entries)
e_above_hull = phase_diagram.get_e_above_hull(entry_of_interest)

print("e_above_hull", e_above_hull)
```

If you have not previously used _pymatgen_, it is a Python code and can be installed using `pip install pymatgen` or `conda install --channel conda-forge pymatgen`.

_Note: the above information for v2019-12-04 is now out of date._

## v2019.11.21

During deployment of the new v2019.11 database, there was temporary issue with generating interactive phase diagrams leading to incorrect formation enthalpies for a small number of chemical systems. This has now been fixed. Data presented on the materials detail pages was unaffected by this issue.

## v2019.11

* Introduced 3,971 new materials
* Amorphous materials added with `amorphous` tag
* Added `theoretical` which is True when the material matches no known experimental structure from ICSD
* Fixed several inconsistency bugs for `band_gap`, piezo tensors, elastic warnings, and total magnetic moment.

## v2019.05

* Introduced a new `deprecated` field to materials. By default the website and API only search for materials that are not deprecated: {“deprecated”: false}.
* Deprecated 15,000 and added 3,600 new materials. We will be recomputing the deprecated materials to fill these spaces back up. Some of these new relaxations may end up matching current materials, so the total number of materials is not guaranteed to be the same as in V2019.02. This also affects downstream properties. Most notably, \~3k elastic tensors associated with the deprecated materials have been removed from the database and are no longer accessible.
* Fixed an issue with sandboxes not properly building the whole hull. Previously, only the sandboxed chemical systems were being recalculated for energy\_above\_hull searches

## v2019.02

* Added over 47,000 new materials from orderings of disordered ICSD as well as compounds from the Pauling File
* Finalized enforcing symmetry on piezo tensors
* Moved third order elastic data to elasticity\_third\_order so that people are not swamped by the mountain of information associated with it.

## v2018.12

* Adjusted the mp-id naming scheme to fix “mvc” ids taking over old mp-ids.
* Fixed piezoeletric max\_direction to be a miller index rather than a unit vector.

## v2018.11

* Changed the grouping of magnetic materials to aggregate all magnetic orderings of a given material into a single material-id, and report the lowest energy ordering
* Fixed incorrect calculation and display of polycrystalline dielectric constants
* Fixed labeling of all materials as high-pressure. Note we’re parsing ICSD tags for this labeling so while some materials may not conventionally be considered high-pressure, a single matching ICSD entry can tag a material as such. We would love to hear comments on how we could better tag high-pressure materials
* Begun enforcing the symmetry of the structure on piezo tensors. In general, this reduces the expected piezo value.
