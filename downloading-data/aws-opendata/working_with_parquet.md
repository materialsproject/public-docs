---
description: >-
  An introduction on how to interact with the json format data hosted on the
  Materials Project AWS Open Data buckets.
---

# Legacy json Data on AWS Open Data

{% hint style="info" %}
All json format data products stored in the buckets of Materials Project's OpenData repositories are considered 'legacy' and candidates for migration to cloud-native data formats. Consult the documentation for [MP's active data products](../../materials-project-data-lakehouse/) for updated usage patterns. &#x20;
{% endhint %}

To run the examples in this section, it will help to use a python environment with `pandas`  and `s3fs` installed

## Structured vs. unstructured data

Much of the raw simulation data that the Materials Project (MP) uses to build up material properties is in plain text format. This is simply because the software used in scientific applications (e.g., VASP, Quantum Espresso, etc.) predates modern data structure standards.

When the extended MP universe began to explore performing high-throughput materials simulations in the early late 2000s, it became clear very quickly that interacting with plain text files would not be practical. `pymatgen` was built around the idea of using JSON to structure raw simulation input and output: all VASP-related objects in pymatgen have a `to_dict` and `from_dict` method to make round-tripping from JSON possible. JSON was chosen partly because it is very simple, compatible with python-native dictionaries, and because it forms the document structure of MongoDB, which was selected for use in orchestrating MP's workflows.

While JSON is supported by many languages, it is not well-suited to data streaming nor for partial retrieval of data. [JSON lines](https://jsonlines.org/) (`.jsonl`) is one attempt to allow for streaming of JSON data. Each line of a JSONL file contains a new JSON object. Much of MP's data up to mid-2025 has been distributed as JSONL for this reason.

## Worked example: JSON

Let's look at the `manifest.jsonl` file which represents the high-level metadata of MP's `summary` data collection. This file is located on MP's OpenData `build` [bucket](https://materialsproject-build.s3.amazonaws.com/index.html#collections/):

```python
import pandas as pd

summary_metadata = pd.read_json(
    "s3://materialsproject-build/collections/2025-09-25/summary/manifest.jsonl.gz",
    lines = True
)
print(summary_metadata.columns)
>>> ['band_gap', 'density', 'deprecated', 'e_electronic', 'e_total', 'energy_above_hull', 'formation_energy_per_atom', 'formula_pretty', 'last_updated', 'material_id', 'nelements', 'sourced_from_path', 'symmetry_number', 'task_ids', 'theoretical', 'total_magnetization']
```

Suppose we wanted to retrieve all materials in MP with a band gap between 0.1 and 1.0 eV, and a hull energy less than 0.05 eV/atom. To do this with the `manifest.jsonl.gz` file, we would need to download the entire file and then filter using the `pandas.DataFrame` shown above. In reality, the only columns we need to do that filtering are: `band_gap`, `material_id`, and `energy_above_hull`.

Suppose now that we did not have the `manifest.jsonl` file, and needed to extract the same information from the entire summary collection. We would have to: 1. Iterate over all JSONL files in the `summary` bucket 2. Download each JSONL file in its entirety 3. Save only the material IDs of those materials matching our filters

```python
import pandas as pd

filtered_mp_ids = set()
base_prefix = "s3://materialsproject-build/collections/2025-09-25/summary/"
for nelements in range(1,10):
    for sgn in range(1,231):
        try:
            df = pd.read_json(
                base_prefix + f"nelements={nelements}/symmetry_number={sgn}.jsonl.gz", lines=True
            )
            filtered_mp_ids.update(
                df[(0.1 < df.band_gap) & (df.band_gap < 1.) & (df.energy_above_hull < 0.05) ].material_id
            )
        except Exception:
            continue
```
