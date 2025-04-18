---
description: Tips for downloading large data sets from the API
---

# Tips for Large Downloads

The Materials Project API imposes rate limits on requests starting at 25/second. Below are a few tips for downloading large datasets quickly without hitting the limit:

1.  **To get data for multiple materials, pass query parameter values as lists where available**. For example, avoid looping over material ID values and instead pass the materials as a list in the `search` method:\


    ```python
    with MPRester("your_api_key_here") as mpr:
        docs = mpr.materials.summary.search(
            material_ids=["mp-149", "mp-13", "mp-22526"]
        )
    ```
2.  **Before requesting data, use the has\_props key to find which materials have data for your desired property.** One source of wasted queries occurs when data is requested for materials that are either nonexistent or do not contain the property of interest. You should instead first determine what materials have the data you are looking for. For example, below is a query to get all of the material ID values for entries that have dielectric and density of states data:\


    ```python
    with MPRester("your_api_key_here") as mpr:
        docs = mpr.materials.summary.search(
            has_props=["dielectric", "dos"], fields=["material_id"]
        )
    ```
3.  **Restrict the data returned to the specific fields of interest, to the extent possible.** This will help our server load and greatly speed up data retrieval. For example, if you are only interested in the material ID, volume, and list of elements you can pass those values to the `fields` argument:\


    ```python
    with MPRester("your_api_key_here") as mpr:
        docs = mpr.materials.summary.search(
            material_ids=["mp-149", "mp-13", "mp-22526"],
            fields=["material_id", "volume", "elements"]
        )
    ```
4.  **For large, long-running, or frequently duplicated queries, we ask that you make a local copy and retrieve the data using the API only once.** This will speed up your own analyses and also avoid unnecessary loads on the Materials Project servers. Additionally, we've optimized downloads of full collections such that they're often more efficient and faster than providing long lists of `material_ids` or `fields`. For instance,\


    ```python
    with MPRester(
        "your_api_key_here", monty_decode=False, use_document_model=False
    ) as mpr:
        docs = mpr.materials.summary.search()
        # save docs to file ...
    ```

We are currently in the process of developing additional tools for users to download extremely large datasets more easily. If you have further questions, please consult [our forum](https://matsci.org/materials-project) for existing solutions and/or post a question specifying in detail the issue you are facing and listing the steps you have taken to try to resolve the issue.
