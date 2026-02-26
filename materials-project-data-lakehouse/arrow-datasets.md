---
description: >-
  MP's python-like interface for arrow-backed data products and (anti)patterns
  for working with arrow data.
---

# Arrow Datasets & the MPDataset Interface

### Arrow Datasets(& Delta Tables)

For very large datasets, it is often impossible (or very unwieldy) to store the data as a single parquet file. An arrow dataset can be used parse the high-level metadata of multiple parquet files to yield a single entry point to the data contained in multiple parquet files. MP's `tasks` collection, in the `parsed` [bucket](https://materialsproject-parsed.s3.amazonaws.com/index.html#core/tasks/) is an arrow dataset (with [Delta](https://docs.delta.io/) on top). MP's python API client, `mp-api`, has tools to retrieve this data in its entirety and paginate through it in a memory-efficient way:

```python
>>> from mp_api.client import MPRester

>>> with MPRester(<YOUR_API_KEY>) as mpr:
...    tasks = mpr.materials.tasks.search()
```

The full `tasks` collection requires >10 GB of on-disk space even in an efficient parquet representation. The client query above will download all tasks to your machine and allow you to load them into memory as they are requested.

{% hint style="info" %}
_A list of endpoints with arrow support and `mp-api` integration can be found at_ [_Supported Data Products_](supported-data-products.md)
{% endhint %}

### MPDatasets

The return type of the `tasks` variable in above snippet is a [`MPDataset`](https://github.com/materialsproject/api/blob/c0f976df68b7996cf2505dbe637d9e76dc76ea20/mp_api/client/core/utils.py#L247) - a thin wrapper around the underlying arrow dataset that has been stored on disk. _To preserve the behavior of any existing user code_, `MPDataset` _objects behave as would be expected from the return value of any other `MPRester` query_, i.e., like a typical iterable container of Pydantic models or python dictionaries. Indexing, slicing, and looping behave accordingly, but warnings will be raised indicating this is sub-optimal usage:

```python
>>> _ = tasks[0]
<stdin>:1: MPDatasetIndexingWarning:
            Pythonic indexing into arrow-based MPDatasets is sub-optimal, consider using
            idiomatic arrow patterns. See MP's docs on MPDatasets for relevant examples:
            docs.materialsproject.org/materials-project-data-lakehouse/arrow-datasets
```

#### A Better Path

Here's a real example retrieving the `structure` field for all r2SCAN documents in the `tasks` dataset (stored on disk as parquet, above snippet) with non-zero bandgaps.&#x20;

* Typical comprehension based filtering (sub-optimal):

```python
# this will finish, eventually...
>>> non_metallic_r2scan_structures = [
    x.structure 
    for x in tasks 
    if x.output.bandgap > 0 and x.run_type == "r2SCAN"
]
```

* And filtering using arrow's compute engine (leveraging concepts mentioned [here](arrow-parquet-and-otfs.md#apache-arrow-parquet)):

```python
>>> import pyarrow.compute as pc
# get pyarrow dataset backing the 'tasks' MPDataset object
>>> tasks_ds = tasks.pyarrow_dataset
# filter expression for predicate pushdown
>>> expr = (pc.field(("output", "bandgap")) > 0) & (pc.field("run_type") == "r2SCAN")
# select 'structure' column for column pruning
# execution time should be a few seconds at most on any reasonable machine
>>> non_metallic_r2scan_structures = tasks_ds.to_table(columns=["structure"], filter=expr)
```

`non_metallic_r2scan_structures` will be a pyarrow Table, but can be de-serialized by calling:

```python
>>> as_py = non_metallic_r2scan_structures["structure"].to_pylist(maps_as_pydicts="strict")
```

PyArrow's syntax and usage patterns may be uncomfortable at first, but the performance benefits gained from delaying de-serialization (arrow -> python) as long as possible and using arrow's compute engine are well worth the intial learning curve. Consult the [PyArrow Cookbook](https://arrow.apache.org/cookbook/py/) for some informative examples of leveraging arrow's strengths.&#x20;

### (De)Serialization Stumbling Blocks

{% hint style="info" %}
_MP's developers are working to solve this issue so users won't have to think about this in the future, but consider this issue a work-in-progress_
{% endhint %}

Historically, all of MP's data products have been stored in MongoDB collections. MongoDB's (and more generically, json's) flexibility is antithetical to the fully structured format of parquet, which led to some workarounds being needed to serialize certain fields of various MP data products.&#x20;

An example is the [`incar`](https://github.com/materialsproject/emmet/blob/324115fa404d37bb7170c95f8570d590fbc98905/emmet-core/emmet/core/vasp/calculation.py#L193) field of a `CalculationInput` , which is an extremely heterogeneous dictionary where the only feasible option for uniformly strictly typing this field was just dumping the dictionary to a string during [serialization](https://github.com/materialsproject/emmet/blob/324115fa404d37bb7170c95f8570d590fbc98905/emmet-core/emmet/core/types/typing.py#L102), and then [re-hydrating](https://github.com/materialsproject/emmet/blob/324115fa404d37bb7170c95f8570d590fbc98905/emmet-core/emmet/core/types/typing.py#L95) accordingly.

All of the Pydantic document models that define the schemas of MP's data products (available for reference in [`emmet-core`](https://github.com/materialsproject/emmet/tree/main/emmet-core)) will handle this seamlessly (with some coercion using pydantic), but again, delay de-serialization as long as possible for the best performance. To extend the example above, let's fully hydrate the `non_metallic_r2scan_structures` filter result as pymatgen structures:

```python
>>> from emmet.core.types.pymatgen_types.structure_adapter import StructureType
>>> from pydantic import TypeAdapter
# first de-serialization: arrow -> python (slow)
>>> as_py = non_metallic_r2scan_structures["structure"].to_pylist(maps_as_pydicts="strict")
# second de-serialzation: python -> pymatgen (also slow)
>>> as_pmg = TypeAdapter(list[StructureType]).validate_python(as_py)
```

This unfortunately incurs two de-serialization hits, but will complete in a reasonable time (order of seconds for this example of 42k entries).
