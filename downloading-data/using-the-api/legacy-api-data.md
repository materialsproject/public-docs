# Legacy API data

### Accessing Legacy Materials Project Structures

\
Older Materials Project (MP) IDs referenced in the literature and MPContribs entries may not be directly accessible via the new API, even with `deprecated=True`. Many of these structures can still be retrieved using the new API's _tasks_ endpoint. The legacy API is also still accessible (as of mid-2025), but its use is discouraged for new work.

#### Recommended: Using the Next-Gen API (`mp-api`) — Tasks Endpoint

To retrieve structures for legacy MPIDs (including `mvc-` and old `mp-` IDs), use the `tasks` endpoint:

```python
from mp_api.client import MPRester

with MPRester() as mpr:
    tasks = mpr.materials.tasks.search(task_ids=['mvc-13350', 'mvc-16068', 'mvc-13133', 'mp-867303']) # Each item in 'tasks' contains the structure and associated information for task in tasks: print(task.task_id, task.structure)
```

**Note:**

* The structure you retrieve may not exactly match the one referenced in older publications.
* The `tasks` endpoint is preferred and supported for retrieving historical structure data.

#### Legacy API (not recommended, will cease operation in September 2025)

If you must access the legacy API (e.g., for reproducibility), you can use an older version of pymatgen:

1.  Install pymatgen 2022.7.25 (or similar):

    ```bash
    pip install 'pymatgen==2022.7.25'
    ```
2.  Query legacy structures (requires legacy API key):

    ```python
    from pymatgen.ext.matproj import MPRester as MPResterLegacy 
    from pymatgen.core import Structure 

    LEGACY_API_KEY = "your_legacy_api_key_here"  # 19-character string 
    query = {"formula": "FeO"}  # Or any other valid query 
    with MPResterLegacy(LEGACY_API_KEY) as mpr:
        materials = mpr.query(query, ["material_id", "cif"], mp_decode=False) 
        structures_by_mpid = { doc["material_id"] : Structure.from_str(doc["cif"], fmt="cif") for doc in materials }
    ```

**Caveats:**

* The legacy API is currently being phased out, and will be retired in September 2025
* There is no official support for the legacy API
* Bulk download of the full legacy database is available as of snapshot [28 October, 2022](https://materialsproject-build.s3.amazonaws.com/index.html#collections/2022-10-28/), and older snapshots may be made available (see [MP AWS downloads](https://materialsproject.org/downloads) for updates).

\
\
