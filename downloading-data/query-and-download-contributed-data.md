---
description: >-
  How to download contributed data from the Materials Project (MP)
  website/database.
---

# Query and Download Contributed Data

## Interactively

{% hint style="danger" %}
Under Construction. We are working on enabling CSV/JSON download buttons for entire projects or specific queries. The downloads will also function as versioned snapshots for each project.
{% endhint %}

## Programmatically

Querying MPContribs data programmatically involves the following steps:

* Install the python package [mpcontribs-client](https://pypi.org/project/mpcontribs-client/).
* Initialize the client with your API key and a project
* Check `client.available_query_params()` for available query parameters
* Build up a query dictionary
* Decide which fields to retrieve
* Run `client.query_contributions()`

Here's an example for the `carrier_transport` dataset:

```graphql
from mpcontribs.client import Client
client = Client(apikey="your-api-key-here", project="carrier_transport")
client.available_query_params()  # print list of available query parameters
query = {"formula__contains": "Au", "data__PF__p__value__lt": 10}
fields = ["identifier", "formula", "data.metal", "data.S.n.value"]

client.query_contributions(
    query=query, fields=fields, sort="-data.S.n.value", paginate=True
)
```

By default, `paginate` is `False` which will only retrieve the first page of results and should be used to test the `query`, `fields` and `sort` parameters before paginating through all results.

If entire projects or large subsets of contributed data are downloaded for later use, it is often more efficient to use the `client.download_contributions()` function. It also takes a `query` as argument and downloads all results as `json.gz` files behind the scenes. Only locally missing data is downloaded when `download_contributions` is run and contributions are loaded from disk. This function always retrieves all fields included in the `data` component, so the `fields` argument is not available/needed. Additional components (i.e. `structures`, `tables`, and `attachments`) can be included in the downloads through the `include` argument:

```graphql
client.download_contributions(query=query, include=["tables"])
```
