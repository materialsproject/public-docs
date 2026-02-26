---
description: >-
  Not all data products provided by the Materials Project are subject to the
  same terms of use
---

# Access-controlled Data

{% hint style="info" %}
A full list of the terms of use applying to each Materials Project data product are available at: [Materials Project - Terms of Use](https://next-gen.materialsproject.org/about/terms)
{% endhint %}

Utilizing the `mp-api` python client for the Materials Project API will seamlessly handle data access according to the terms of use you have agreed to upon account registration (and as you agree to further terms, e.g., opting into using the GNoME dataset for [non-commercial](https://next-gen.materialsproject.org/about/terms#gnome) uses).&#x20;

Retrieving data directly from the Materials Project's data lakehouse will circumvent the data access control convenience features provided by `mp-api` python client. This is perfectly acceptable, but know that you as a user are still responsible for abiding by the Materials Project's terms of use provided above.

For convenience, a list of relevant metadata filtering methods (similar to the methods used in `mp-api`) are provided here for each of the data products in the Materials Project that have special access restrictions

### Graph Networks for Materials Exploration Database (GNoME) <a href="#gnome" id="gnome"></a>

The GNoME dataset is subject to the following terms: [GNoME Database License and Terms of Use](https://next-gen.materialsproject.org/about/terms#gnome)

A subset of the data contained in following data products endpoints are effected by this set of terms: [tasks](https://materialsproject-parsed.s3.amazonaws.com/index.html#core/tasks/), [tasks-build](https://materialsproject-parsed.s3.amazonaws.com/index.html#core/tasks-build/), [summary](https://materialsproject-build.s3.amazonaws.com/index.html#collections/summary/), and [materials](https://materialsproject-build.s3.amazonaws.com/index.html#collections/materials/)

The following list of [`batch_id`](https://github.com/materialsproject/emmet/blob/324115fa404d37bb7170c95f8570d590fbc98905/emmet-core/emmet/core/base.py#L26-L28)s correspond to documents associated with the GNoME dataset:

```
["gnome_r2scan_statics"] 
```

So if for example you have a full copy of the `tasks` table (or `tasks-build`) stored locally as an arrow dataset, the following snippet could be used to create a new dataset with all GNoME entries removed:

```python
import pyarrow.dataset as ds
import pyarrow.compute as pc

gnome_batch_ids = ["gnome_r2scan_statics"]

dataset_with_gnome = ds.dataset(<PATH TO LOCAL DATASET>)
filter_expr = pc.invert(pc.field("batch_id").isin(gnome_batch_ids))
no_gnome_dataset = dataset_with_gnome.filter(filter_expr)
# write new dataset, sort, groupby, etc.
...
```

A similar process can be applied to the `summary` and `materials` tables, all that needs to be adjusted is the field name in the filter expression:

```python
filter_expr = pc.invert(pc.field("builder_meta.batch_id").is_in(gnome_batch_ids))
```

