# Website Changelog

#### 2022-12-16 (7ca3bcd3)

* Fix issue with API query, see [here](https://matsci.org/t/rest-query-returned-with-error-status-code-500/45793).
* better integration of MPRester and MPContribs API python clients.\
  **Please upgrade to `mp-api==0.30.5` and `mpcontribs-client==5.0.4`**

#### 2022-12-02 (13f229ed)

* Fix an incorrect unit label for elasticity data on the new website. Thank you to Serge Maalouf for reporting.
  * Data returned from the API was correct and unaffected by this error.
* Fix for insufficient precision in reporting atomic co-ordinates of some materials. Kindly reported by Branton Campbell for the entry `mp-1106336`.
  * Data returned from the API was correct and unaffected by this error.
* An issue with displaying "task detail" pages is resolved.

#### 2022-08-09 (f2aa3e0a)

* Resolved a bug with "MOF Explorer" detail pages not loading.
* We are investigating an issue with the "Crystal Toolkit" app.
  * This was resolved.

#### 2022-07-28 (e7527896)

* Added "Alloy Systems" section to the material details pages.
  * This is a preview of a new feature and is not yet peer-reviewed.
  * More information on the methodology is available [here](https://arxiv.org/abs/2206.10715).
  * Examples of this feature might be seen on the materials detail page for [CdTe](https://materialsproject.org/materials/mp-406) or [GaN](https://materialsproject.org/materials/mp-804).

#### 2022-07-12 (5d802243)

* Fixed an issue with permuted axis labels in the Equations of State plots, kindly reported by [zzyfor2019](https://matsci.org/u/zzyfor2019) on the forum
  * The data returned by the API was correct and unaffected by this error
* Fixed an issue with swapped labels in the Battery Explorer, kindly reported by 施荣鑫 via email
  * The data returned by the API was correct and unaffected by this error
