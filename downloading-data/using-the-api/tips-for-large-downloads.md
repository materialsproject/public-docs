---
description: Tips for download large datasets from the API
---

# Tips for Large Downloads

The Materials Project API imposes rate limits on requests starting at 25/second. If you are blocked by our servers, please contact **support@materialsproject.org**.&#x20;

Below are a few tips for downloading large datasets quickly without hitting the limit:

1.  **Always pass query parameter values as lists where available**. For example, instead of looping over material ID values and using `get_data_by_id` instead pass them as a list in the `search` method:\


    ```python
    with MPRester("your_api_key_here") as mpr:
        docs = mpr.summary.search(material_ids=["mp-149", "mp-13", "mp-22526"])
    ```
2.  **Query initially using has\_props to find materials with specific properties.** For example, below is a query to get all of the material ID values for entries that have dielectric and density of states data:\


    ```python
    with MPRester("your_api_key_here") as mpr:
        docs = mpr.summary.search(has_props=["dielectric", "dos"], 
                                  fields=["material_id"])
    ```
3.  **Ask for specific fields as much as possible.** This will help our server load, and greatly speed up data retrieval. For example, if you are only interested in the material ID, volume, and list of elements you can pass those values to the `fields` argument:\


    ```python
    with MPRester("your_api_key_here") as mpr:
        docs = mpr.summary.search(fields=["material_id", "volume", "elements"])
    ```

We are currently in the process of developing additional tools for users to download extremely large datasets more easily. If you think the amount of data you would like falls into this category (e.g. all data from many collections, or a large number of charge densities or band structures) please contact **support@materialsproject.org** specifying the details.
