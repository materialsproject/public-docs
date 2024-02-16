---
description: MP data is also available through the AWS OpenData Program.
---

# AWS OpenData

In an effort to make our data as accessible as possible (FAIR principle) as well as significantly improve data downloads and take pressure off our servers, we are making a growing list of our data products available through the [AWS OpenData Program](https://aws.amazon.com/opendata). Also see the entries for MP-managed data on the [AWS OpenData Registry](https://registry.opendata.aws/materials-project/) or the [AWS Data Exchange](https://aws.amazon.com/marketplace/pp/prodview-hc3sh3u5ukiya).

## Overview

MP data is organized in 3 buckets named `materialsproject-{raw,parsed,build}`. Note that the particular organization of our data in these buckets is still in flux and can change without notice as we integrate them into our cloud infrastructure.

### raw data

{% hint style="info" %}
We are in the process of providing VASP output files for our calculations in the `raw` bucket. Look out for announcements through our email lists and notifications on our website.
{% endhint %}

### parsed data

The [`parsed` bucket ](https://materialsproject-parsed.s3.amazonaws.com/index.html)contains objects that MP generates by parsing the VASP output files. The objects form the basis for our builder pipelines which create the derived high-level data collections served through the MP API and website. All S3 objects in this bucket are serialized `pymatgen` or `emmet` python objects and most are stored as gzip-compressed JSON files for each MP ID (i.e. `<prefix>/<mp-id>.json.gz`). We are in the process of grouping documents into JSON Lines (JSONL) files to reduce the number of files and significantly improve transfer speeds. `tasks` are now organized by `nelements/output.spacegroup.number` and a timestamp (`dt`) derived from the earliest `completed_at` in the list of tasks included in the respective object.&#x20;

| prefix            |  # objects  |     size    |
| ----------------- | :---------: | :---------: |
| `/dos`            |     691k    |   63.1 GB   |
| `/bandstructures` |     705k    |    1.4 TB   |
| `/chgcars`        |     400k    |    7.2 TB   |
| `/aeccar{0,1,2}s` | 138.7k each | 1.1 TB each |
| `/elfcars`        |    107.5k   |    101 GB   |
| `/locpots`        |     158k    |    2.5 TB   |
| `/tasks`          |     1556    |    34 GB    |

### build data

The [`build` bucket](https://materialsproject-build.s3.amazonaws.com/index.html) contains the high-level derived data that comprises the source for the collections available through the [MP API](https://api.materialsproject.org/) as well as pre-built objects and images for efficient visualization on the website.

The collections and pre-built objects are versioned by the database release date and individual documents grouped into gzip-compressed JSONL files. Images are stored in PNG format. Use the `ls` command for the AWS CLI or the [bucket explorer](https://materialsproject-build.s3.amazonaws.com/index.html) to list the categories available under each prefix (see [download section](aws-opendata.md#explore-and-download-data) below).

| prefix         | version       | # objects |   size  |
| -------------- | ------------- | :-------: | :-----: |
| `/collections` | `/2022-10-28` |   12.6k   |  2.8 GB |
|                | `/2023-11-01` |   18.4k   |  6.1 GB |
| `/objects`     | `/2022-10-28` |    289k   | 55.9 GB |
| `/images`      | N/A           |    200k   |  58 GB  |

## Explore & Download Data

We are in the process of integrating all available data into the `mp-api` python client for improved convenience and efficiency. However, all data in MP's OpenData buckets can always be downloaded directly using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Start by exploring the contents of the bucket you're interested in, by either navigating to the bucket's web interface (e.g. [https://materialsproject-parsed.s3.amazonaws.com/index.html](https://materialsproject-parsed.s3.amazonaws.com/index.html)) or using the CLI's `ls` command:

```sh
aws s3 ls --no-sign-request s3://<bucket>/<prefix>/[<version>]/

# examples
aws s3 ls --no-sign-request s3://materialsproject-parsed/
aws s3 ls --no-sign-request s3://materialsproject-build/
aws s3 ls --no-sign-request s3://materialsproject-build/collections/2022-10-28/
aws s3 ls --no-sign-request s3://materialsproject-build/images/
```

All objects for a prefix can be downloaded, using the format

```sh
# the AWS CLI will parallelize download requests automatically
aws s3 cp --no-sign-request --recursive s3://<bucket>/<prefix>/ <output-dir>/

# examples
aws s3 cp --no-sign-request --recursive s3://materialsproject-parsed/tasks/ mp-tasks/
aws s3 cp --no-sign-request --recursive \
    s3://materialsproject-build/images/structures/ mp-images-structures/
```
