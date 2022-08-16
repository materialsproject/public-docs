---
description: >-
  An introduction on how to query for data with the Materials Project API
  client.
---

# Querying Data

There are two main ways of querying Materials Project data. The first is through a specific **Materials Project ID (e.g. mp-149)**, and the second is through **property filters (e.g. band gap less than 0.5 eV).**

Most material property data is available as **summary data** for a specific material. **To query summary data with a single Materials Project ID** use the generic `get_data_by_id` function:

```python
with MPRester("your_api_key_here") as mpr:
    doc = mpr.summary.get_data_by_id("mp-149")
```

**For multiple Materials Project IDs** the `search` method should be used and is much faster:

```python
with MPRester("your_api_key_here") as mpr:
    docs = mpr.summary.search(material_ids=["mp-149", "mp-13", "mp-22526"])
```

The above returns `MPDataDoc` objects with data accessible through their attributes. For example, the Material ID and formula can be obtained for a particular document with:

```python
mpid = doc.material_id
formula = doc.formula_pretty
```

A list of available property fields can be obtained by examining one of these objects, or from the MPRester with:

```python
list_of_available_fields = mpr.summary.available_fields
```

**To query summary data with property filters** use the `search` function. Filters for each of the fields in `available_fields` can be passed to it, returning a list of `MPDataEntry` objects. For example, below is a query to find materials containing `Si` and `O` that have a band gap greater than `0.5 eV` but less then `1.0 eV`.

```python
with MPRester("your_api_key_here") as mpr:
    docs = mpr.summary.search(elements=["Si", "O"], 
                              band_gap=(0.5, 1.0))
```

**Note that by default ALL available property data within `MPDataEntry` objects will be populated.** If one is only interested in a few properties, limiting what data is returned will speed up data retrieval. Pass a list of the fields you are interested in to `fields` to accomplish this. For example, if we were only interested in `material_id`, `band_gap`, and `volume` for the materials from the above query, we could instead use:

```python
with MPRester("your_api_key_here") as mpr:
    docs = mpr.summary.search(elements=["Si", "O"], 
                              band_gap=(0.5, 1.0),
                              fields=["material_id", 
                                      "band_gap", 
                                      "volume"])
```

Now, only the `material_id`, `band_gap`, and `volume` attributes of the returned `MPDataEntry` objects will be populated. A list of available fields that were not requested will be returned in the `fields_not_requested` parameter.

```python
example_doc = docs[0]

mpid = example_doc.material_id       # a Materials Project ID
formula = example_doc.formula_pretty # a formula
volume = example_doc.volume          # a volume

example_doc.fields_not_requested     # list of unrequested fields
```



### Other Data&#x20;

Not all Materials Project data for a given material can be obtained from the summary API endpoint. To access the remaining data, other endpoints must be used. **For a complete list of endpoints see the** [**main API page on the website.**](https://next-gen.materialsproject.org/api)&#x20;

For example, the `initial_structures` used in calculations producing data for a specific material can only be found in the `materials` endpoint. To obtain the `initial_structures` for `mp-149`, the same `get_data_by_id` function can be used:

```python
with MPRester("your_api_key_here") as mpr:
    doc = mpr.materials.get_data_by_id("mp-149", fields=["initial_structures"])

initial_structures = doc.initial_structures
```

Materials also has a search function that can be used with property filters:

```python
with MPRester("your_api_key_here") as mpr:
    docs = mpr.materials.search(elements=["Si", "O"], 
                                band_gap=(0.5, 1.0),
                                fields=["initial_structures"])
                                              
example_doc = docs[0]
initial_structures = example_doc.initial_structures
```

The same querying procedure shown above can be applied to most endpoints. See [advanced usage](advanced-usage.md) and [examples](examples.md) for more information.

### Convenience Functions

In addition to the `get_data_by_id` and `search` functions, there are a small number of top level convenience functions for frequently made queries. These aim to provide a simpler interface, and make use of `get_data_by_id` and `search` under the hood. A couple examples of common queries include obtaining the structure or density of states of a particular material. **These convenience functions are illustrated in the** [**examples section**](examples.md)**.**&#x20;

{% tabs %}
{% tab title="Relevant Code Links" %}
{% embed url="https://github.com/materialsproject/api/blob/main/mp_api/client/core/client.py#L820-L883" %}
Generic `get_data_by_id` method
{% endembed %}

{% embed url="https://github.com/materialsproject/api/blob/main/mp_api/client/routes/materials.py#L59-L184" %}
Materials search method
{% endembed %}

{% embed url="https://github.com/materialsproject/api/blob/main/mp_api/client/routes/summary.py#L34-L293" %}
Summary search method
{% endembed %}
{% endtab %}
{% endtabs %}
