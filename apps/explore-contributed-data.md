---
description: >-
  Walkthrough for exploring contributed data on the Materials Project (MP)
  website.
---

# Explore Contributed Data

![Contributions displayed in dedicated section on materials detail page for mp-2715](https://lh5.googleusercontent.com/67Y4wioD58AQxxz2KpFAEyRcrwVfoHDYzfjE4Wib820v29eDJXK7-VSnrxBKM-F95r4CtCyR3gfgnsHFNfFLHNm6xueiwYcSIm8Bupg0YIBWmFKAWxnE-VtKDEG8U\_j3CHY4Qc3Hl2M)

Contributed data from MPContribs projects are included in the materials details pages on MP for every contribution that uses an mp-id as its `identifier`. Public contributions in public projects are shown to all MP users whereas private projects or private contributions in public projects are only shown to the project owner and their collaborators. A summary of each contribution to a material is summarized in form of a contribution card containing meta-data in form of project info, data columns and download links to the other contribution components `structures`, `tables`, and `attachments` if available. The project title is linked to its project landing page in the MPContribs portal and the contribution ID is linked to the according contribution detail page.

![Main page of the default MPContribs deployment at https://contribs.materialsproject.org](<../.gitbook/assets/Screen Shot 2022-07-28 at 3.34.55 PM.png>)

The main page of an MPContribs deployment contains an entry for every contributed dataset/project with their titles linking to their respective landing pages. Each entry also includes the project's publication status, its authors, and statistics about their contents. A search box is provided to filter project titles, authors, and descriptions by keywords and reduce the list accordingly. The dropdowns in the column headers can be used to filter the list of projects by values in each column.

![MPContribs interface to search across all contributions and display results in list and card form](https://lh4.googleusercontent.com/1aijY5cGF3ipCxb3EOSE09VhdozSqTUkTfQpv-k-ixE9qkuKGqUG9kMhqIH9OViBYkWxv\_DJvrzXP0hFJydHtfJSboLYY\_emQcUVNIyYWw5v1BeTElToSHxvGP0X8pxh0bFaXzden4k)

The MPContribs search interface allows for filtering and previewing contributions across all materials and projects. Projects, formulas and identifiers can be selected to generate a list of results. The selected result is displayed in form of a contribution card. The title of the card links to the project's landing page, and the _Details/Info_ button opens the corresponding detail page. Authors and description are expandable, and references linked above the title. Each contribution card also contains its hierarchical data.

![Project landing page for the "Carrier Transport" dataset](<../.gitbook/assets/Screen Shot 2022-07-28 at 4.22.15 PM.png>)

Each project/dataset has a _landing page_ with a unique URL on the MPContribs Portal. Upon approval of the project by MP, a Digital Object Identifier (DOI) can be assigned to the landing page for citation in journals. This page thus serves as entry point for contributors to share their data with their community and refer to an overview of their dataset in journals, for instance. In addition to  title, description, authors and references, a landing page also contains a generic overview table listing all the project's contributions to MP materials (or alternatively formulas or chemical systems). This default table provides a few important functionalities out of the box:

* **Search Box**: Filter the list of contributions by searching for specific sub-strings in the identifier or formula columns. Any other string-type columns in the `data` component can be filtered, too.
* **Grouped Headers**: Nested fields in the `data` component automatically appear grouped in the table header. Units are also pulled into header next to the column name.
* **Column Sort**: By default, the table is sorted by contribution insertion order (`natural`). Any other column can be used for the table sort by clicking on the column name. Repeated clicks cycle through ascending, descending and natural order.
* **Pagination**: Contributions are paginated. The pages can be iterated by scrolling to the bottom of the table.
* **MP Details Pages**: If the `identifier` column contains an MP material ID or formula, the cell will automatically link to the according materials details or disambiguation pages.
* **Contribution Details Pages**: The `id` links to the details page for a contribution containing a rendered version of the full database entry.
* **Column Manager**: Table columns to be shown or hidden can be selected via simple dropdown menu.
* **Components**: the lists for each contribution component are rendered as tab containing a link to reveal their meta-data and to download a specific `structure/table/attachment`.
* **Downloads**: Archives with contributions in CSV or JSON format can be downloaded either for the entire project with or without a selection of components, or for a subset of contributions by first filtering the contributions to include.

![MOF Explorer serving data stored in the qmof MPContribs project](https://lh6.googleusercontent.com/OPY-cDRcWmGTBwAlLZ3uOUnaa-Jo5bvLMkliYDHsWr02qLfxi2g230bmqTqcCqPACV2XBPs4jD9G3QR\_3ILT5eZvAmBBFzLmMSp\_O1qwDp60QbaDhWMy\_RZMHGwkEaifgAHEnA6oU6s)

There are also dedicated explorers available for the MPContribs projects `qmof` and `open_catalyst_project` which provide an alternative view/interface for the project's data that's integrated with the Materials Project website. These explorers will likely become the default for all MPContribs projects in the future.

![](https://lh3.googleusercontent.com/RZXPIC-azpXUx0zxFGYO28w50EG1IwsHBQMkxUsXPgopo7RdcKMQ0noBrKIQUO-VlO3qEu\_dmlDiNaqwDSx6o--wm3nbg0cB5CF4DvixkD8vEzKPezI67IFjlOI8Xj9D6FDroxeYBLo)

![](https://lh6.googleusercontent.com/5FQQY4etNAFt7mBPh1G4t-CHHbGge2eOmFE1CpnVl7JxsFtLcO76djIS50dnr4-xmmWN3YVmaEs-UnAnHR3JD13PRHwSaGVJygUB\_5fJbUn\_MyNkgfHpCTNOzArqFIeWdcVTDqJLUiI)

Each contribution in a dataset is assigned a unique identifier (see MongoDB [ObjectId](https://docs.mongodb.com/manual/reference/method/ObjectId/)) which can be used to access its _Contribution Details Page_ rendering an interactive version of its full content. The detail page is a static version of a fully functional [Jupyter](https://jupyter.org/) notebook using the MPContribs [API Client](https://pypi.org/project/mpcontribs-client/) python library. It contains code showing how to interact with a contribution programmatically along with the resulting output. The navigation bar at the top provides links to jump to a respective component and a download button to retrieve the contribution in JSON format.

