---
description: >-
  How to contribute your own user-created data to the Materials Project (MP)
  website/database.
---

# Contribute Data

### Interactively

{% hint style="danger" %}
Under Construction. We are working on creating interactive interfaces that enable our user to contribute data in addition to the programmatic approach below.
{% endhint %}

### Programmatically

#### Quickstart

Read the [concepts section](../mpcontribs.md#concepts), [Create a project](https://contribs.materialsproject.org/contribute), install [`mpcontribs-client`](https://pypi.org/project/mpcontribs-client/), and set the `MPCONTRIBS_API_KEY` environment variable to the API key shown on the [profile page](https://profile.materialsproject.org) or your [dashboard](https://materialsproject.org/dashboard). The following code snippet outlines the general process of adding data to your project. See the next section for step-by-step instructions.

```python
from mpcontribs.client import Client
client = Client(project="my_test_project")
client.init_columns({"a": "eV", "b.c": None, "b.d": ""})
contributions = [{
    "identifier": "mp-4",
    "data": {
        "a": "3 eV",
        "b": {"c": "hello", "d": 3}
    },
    "structures": [`pymatgen.core.Structure`, ...],
    "tables": [`pandas.DataFrame`, ...],
    "attachments": [`pathlib.Path`, `mpcontribs.client.Attachment`, ...]
}]
client.submit_contributions(contributions)
```

#### Step-by-step instructions

The general process of preparing and contributing data using the [mpcontribs-client python library](https://pypi.org/project/mpcontribs-client/) follows the steps below. Make sure to read the [Concepts section](../mpcontribs.md#concepts) before continuing.

**Install dependencies.** You might not need all of them depending on your use case:\
`pip install mpcontribs-client mp_api pandas flatten_dict pymatgen monty`

**Import commonly used libraries** (some might be optional in your case):

```python
import json
import os
import gzip

import pandas as pd
import numpy as np

from glob import glob
from pathlib import Path
from math import isnan
from flatten_dict import flatten, unflatten
from collections import defaultdict

from monty.serialization import loadfn
from pymatgen.core import Structure
from mp_api.client import MPRester
from mpcontribs.client import Client, Attachments
```

[Initialize `MPRester`](../downloading-data/using-the-api/getting-started.md) and **create a project.** Take an extra minute to think about the `name` for your project. It needs to be 3-31 characters long and can only use alpha-numeric characters or the underscore (`_`). You'll use it to refer to your project in the python client, and it'll be used as part of the URL for your project's landing page.\
\
You can either create the project by [completing this form](https://contribs.materialsproject.org/contribute) or programmatically as shown below. Replace the example values below with the info pertinent to your project. Projects created with these example values or with insufficient information will be removed immediately. Also check out [the doc strings](https://jakevdp.github.io/PythonDataScienceHandbook/01.01-help-and-documentation.html) for more info about any functions used here.

```python
with MPRester() as mpr:  # needs MP_API_KEY environment variable to be set
    mpr.contribs.create_project(
        name="my_test_project",
        title="My Test Title",
        authors="P. Huck, K. Persson",
        description="Describe the contents of your project here.",
        url="https://example.com/projects/test"
        # URL could be group website, publication DOI, Github repo, ...
    )
```

After the project is created, you can use the MPContribs client directly to exclusively interact with your project going forward:

```python
# set MPCONTRIBS_API_KEY environment variable first (same as MP_API_KEY)
client = Client(project="my_test_project")
client.get_project()
```

**Update submitted project information** if needed. For instance, we might want to add another first author to `authors` or change the label for the default reference and add another URL to `references`:

```python
client.update_project({
    "authors": "P. Huck / J. Munro, K. Persson",
    "references": [
        {"label": "Website", "url": "https://example.com/projects/test"},
        {"label": "PRL", "url": "https://doi.org/123456"}
    ]
})
client.get_project(fields=["name", "authors", "references"])
```

**Load the data** you plan to contribute from disk. As an example,

*   we load the following CSV file (`main.csv`) with each line containing the values intended for the queryable columns in the `data` component for a material. Use `mpr.find_structure()` and/or `mpr.summary.search()` to identify MP materials matching your structure, if possible. Only contributions with MP IDs as `identifier` will show up on MP's materials details pages.

    ```csv
    identifier,energy,polarization,max_distance,polar_icsd,polar_spacegroup
    mp-4,3.5,2,4,5.6,7
    mp-100,15,33,22.4,16,8
    ```
*   we load a list of associated spectra intended for the `tables` component from a folder name `spectra` with the following contents:

    ```
    spectra/
    |- mp-4.csv
    |    energy,XAS,XMCD
    |    1,3,5
    |    2,4,6
    |- mp-100.csv
    |    [...]
    ```
* we load a list of associated CIF files intended for the `structures` component as [pymatgen structures](https://pymatgen.org/usage.html) (folder named `structures` contains a list of CIF files named by MP ID).
*   we load data intended for the `attachments` component directly from files/images on disk using the following directory structure. Attachments can also be created dynamically from standard python lists or dictionaries. See the use of `Attachments.from_list()` below or its doc string for more details.

    ```
    attachments/
    |- mp-4/
    |  |- image.jpg
    |  |- extra.json
    |  |- info.txt
    |- mp-100/
    |  [...]
    ```

```python
main = pd.read_csv("main.csv", index_col="identifier")
tables = {  # map mp-id to pandas DataFrame
    path.stem: pd.read_csv(path, index_col="energy")
    for path in Path("spectra").glob("*.csv")
}
structures = {
    path.stem: Structure.from_file(path)
    for path in Path("structures").glob("*.cif")
}
attachments = defaultdict(list)
for path in Path("test/attachments").rglob("*"):
    if path.is_file():
        attachments[path.parent.name].append(path)
```

**Initialize the columns** for the queryable `data` component of your contributions. This involves deciding on MPContribs-compatible field names, appropriately grouping / categorizing related columns, setting their units, and adding descriptions to be included in the project info.

* Use dot (`.`) notation to group/nest columns up to 4 levels deep. Most non-alphanumeric characters are disallowed (including underscores and spaces) to encourage better data organization by grouping columns and using short, readable, and type-able column names. Use a good description to explain column names, and use the pipe (`|`) character to indicate conditions for a column (e.g. `max`, `300K`, ...) where nesting might not be desired. Falling back on CamelCase is also an option for column naming.
* The column unit can either be an empty string (`""`) to indicate a dimensionless number, a string representing a unit supported by [pint](https://pint.readthedocs.io/), or `None` to indicate that the column values are not numerical.

```python
columns_map = {
    "energy": {"name": "energy", "unit":"eV/atom", "description": "Formation Energy"},
    "polarization": {"name": "polarization", "unit": "µC/cm²", "description": "..."}},
    "max_distance": {"name": "distance|max", "unit": "Å", "description": "..."},
    "polar_icsd": {"name": "polar.icsd", "unit": "", "description": "..."},
    "polar_spacegroup": {"name": "polar.spacegroup", "description": "..."}}
}
columns = {col["name"]: col.get("unit") for col in columns_map.values()}
client.init_columns(columns)
```

**Prepare the list of contributions.**

* The preferred `data` format is to provide numerical data as strings with units rather than naked numerical values. This will be parsed by the API to yield a value and a unit for display and promotes the correct reporting of significant figures.
* If you're trying to include `list`objects in the `data` component, first convert them into dictionaries by giving each element in the list a name. For instance, in the case of tensors, convert `[[1, 2], [3, 4]]` to `{"e11": 1, "e12": 2, "e21": 3, "e22": 4}`. This will make the tensor components queryable.

```python
contributions = []

for record in main.to_dict("records"):
    clean = {}
    for k, v in record.items():
        # remove NaNs (tip: skip any unset/empty keys)
        if not isinstance(v, str) and isnan(v):
            continue
        # convert boolean values to Yes/No, and append units       
        key = columns_map[k]["name"]
        unit = columns_map[k].get("unit")
        val = v
        if isinstance(v, bool):
            val = "Yes" if v else "No"
        elif unit:
            val = f"{v} {unit}"
    
        clean[key] = val

    identifier = clean.pop("identifier")
    contrib = {"identifier": identifier}
    contrib["data"] = unflatten(clean, splitter="dot")
    
    table = tables.get(identifier)
    if table:
        # set table attributes for plotting (see Plotly Express)
        table.attrs = {
            "name": "absorption coefficients",
            "title": "Energy-dependent Absorption Coefficients",
            "labels": {
                "value": "absorption coefficient [cm⁻¹]",
                "variable": "method"
            }
        }
        contrib["tables"] = [table]
        
    structure = structures.get(identifier)
    if structure:
        structure.name = "optional structure name"
        contrib["structures"] = [structure]
        
    attms = attachments.get(identifier)
    if attms:
        contrib["attachments"].from_list(attms)
```

**Submit the contributions and publish the project** when ready.

```python
client.submit_contributions(contributions)
client.init_columns(columns) # shouldn't be needed but ensures all columns appear
# client.delete_contributions()  # remove all contributions from project
client.make_public()
```

### API Docs Page

The [mpcontribs-client python library](https://pypi.org/project/mpcontribs-client/) interacts with the [MPContribs API](https://contribs-api.materialsproject.org) to programmatically access or retrieve experimental and theoretical data contributed by the MP community. Project information is retrievable through the `projects` resource, and the corresponding contributed data through the `contributions` resource. Each project can contain many contributions for an MP material or composition. Each contribution in turn consists of four (optional) components: free-form hierarchical data, tabular data, crystal structures, and attachments. There are separate dedicated resource endpoints for `tables`, `structures`, and `attachments`. See the [Concepts section](../mpcontribs.md#concepts) for more details. Check out the "Models" section on the [API Docs](https://contribs-api.materialsproject.org) page for descriptions of available fields and to try out the API in the browser.
