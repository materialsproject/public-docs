---
description: Leaving Python land
---

# Leveraging External Query Engines

An additional benefit of MP's data being [publicly accessible](../downloading-data/aws-opendata/) in a format like parquet (with [Delta](https://docs.delta.io/) on top) is the new level of freedom that you as a user now have for interacting with MP's data products and implementing custom pipelines that may not be readily available through the `mp-api` Python client.&#x20;

{% hint style="info" %}
_Remember to familiarize yourself with the various_ [_Terms of Use_](https://next-gen.materialsproject.org/about/terms) _and_ [_access-restrictions_](access-controlled-data.md) _for each of the data products in MP's data lake. Interacting with MP's data at the "bare-metal" level removes all guardrails implemented in the `mp-api`_&#x20;
{% endhint %}

### DuckDB

[DuckDB](https://duckdb.org/) is a popular in-process database/query engine with many [language bindings](https://duckdb.org/docs/stable/clients/overview). DuckDB also has [support](https://duckdb.org/docs/stable/core_extensions/delta) for the Delta Lake format, making DuckDB an ergonomic way of interacting with MP's data lake.&#x20;

Here is a simple, somewhat contrived, pipeline for retrieving all DOS available in the Materials Project for hexagonal materials that have been calculated with a GGA+U `run_type`:

{% hint style="info" %}
_Query execution speed will be limited by network speeds when interacting with remote data_
{% endhint %}

```sql
-- saved as gga_hex.sql
-- Install and load the delta extension
INSTALL delta;
LOAD delta;

-- MP's data is in a public bucket, create
-- simple secret w/ the correct region
CREATE SECRET (
    TYPE s3,
    REGION 'us-east-1'
);

-- Create some views for ease of reference later
CREATE VIEW mp_tasks AS
    SELECT * FROM delta_scan('s3://materialsproject-parsed/core/tasks/');

CREATE VIEW mp_dos AS
    SELECT * FROM delta_scan('s3://materialsproject-parsed/core/electronic-structure/total-dos/');

-- Store all hexagonal GGA+U task_ids
SET VARIABLE gga_hex_task_ids = (
    SELECT list(task_id)
    FROM   (
        SELECT task_id,
               run_type,
               symmetry.crystal_system AS crystal_system
        FROM   mp_tasks
        WHERE  run_type = 'GGA+U'
          AND  crystal_system = 'Hexagonal'
    ) AS hex_gga
);

-- Query DOS table, store results in local parquet file
COPY (
    SELECT *
    FROM   mp_dos
    WHERE  identifier IN getvariable('gga_hex_task_ids')
) TO 'gga_hex_dos.parquet.zstd' (FORMAT parquet, COMPRESSION zstd);
```

Which can be ran by invoking the DuckDB CLI:

```bash
duckdb < gga_hex.sql
```

and will return a parquet file with around 3800 DOS (in the [format](https://github.com/materialsproject/emmet/blob/324115fa404d37bb7170c95f8570d590fbc98905/emmet-core/emmet/core/band_theory.py#L326) MP uses for storing DOS entries).

### More Options

Depending on your infrastructure/interests the data products in MP's data lake are compatible with any query engine that speaks Delta Lake, some examples: [Spark](https://docs.delta.io/quick-start/), [Trino](https://trino.io/docs/current/connector/delta-lake.html), [BigQuery](https://docs.cloud.google.com/bigquery/docs/create-delta-lake-table), [Athena](https://docs.aws.amazon.com/athena/latest/ug/delta-lake-tables-querying.html), [Fabric](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-and-delta-tables).

If you develop something cool feel free to reach out and share it with the community!
