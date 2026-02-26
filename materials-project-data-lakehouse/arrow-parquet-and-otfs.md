# Arrow, Parquet, and OTFs

### Apache Arrow / Parquet

{% hint style="info" %}
_A cursory search will turn up extensive literature (engineering blogs, forum posts, docs, etc.) describing the benefits of columnar data formats for cloud-based environments, where efficient data transfer and read throughput have direct implications on cloud spend. For a more technical deep-dive, consult the relevant_ [_Arrow_](https://arrow.apache.org/docs/format/Intro.html) _and_ [_Parquet_](https://parquet.apache.org/docs/file-format/) _specifications/documentation._
{% endhint %}

As a user of the Material Project's arrow-backed data products, the most important concepts to understand and keep in mind are _predicate pushdown_ and _column pruning_, or in lay-terms _reading only what you need_.&#x20;

{% hint style="info" %}
_Many parquet query tools support SQL or SQL-like syntax, though programmatic APIs (e.g., PyArrow, Polars) are equally common._
{% endhint %}

* Predicate Pushdown: A `WHERE`/filter condition (or `$match` if you have a MongoDB-background) that allows your chosen parquet reader to efficiently skip [row groups](https://parquet.apache.org/docs/concepts/). As an example, say I want to query a parquet-based table of [`task`](https://github.com/materialsproject/emmet/blob/324115fa404d37bb7170c95f8570d590fbc98905/emmet-core/emmet/core/tasks.py#L276) documents with calculation `output` s (a struct-type field) that have a `bandgap` greater than 0.1:

```sql
SELECT *
FROM   tasks_table
WHERE  output.bandgap > 0.1
```

* Column Pruning: Refers to simply `SELECT`ing (mongo -> `$project`) which columns you need. Since parquet is a columnar format, rather than reading everything into memory and then selecting the columns you care about, let your chosen parquet reader skip the columns you don't care about. Using the same predicate as before, let's say I only care about the `output` struct's `structure` field:

```sql
SELECT output.structure as structure,
       output.bandgap as bandgap
FROM   tasks_table
WHERE  output.bandgap > 0.1
```

Various examples using actual Materials Project data can be found in [Leveraging External Query Engines](leveraging-external-query-engines.md).

### Open Table Formats (OTFs)

Since parquet files are simply just files, the need for a data management abstraction on top of raw files led to the development of several open table formats (OTFs), including Apache Iceberg, Delta Lake, and Apache Hudi. The Materials Project's chosen table format is [Delta](https://delta.io/), but this may change in the future. A more in-depth discussion of OTFs and their benefits can be found [here](https://delta.io/blog/open-table-formats/).

