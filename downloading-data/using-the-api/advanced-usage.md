---
description: Information for advanced users of the API.
---

# Advanced Usage

In addition the `search`, an alternative approach to querying data is with the underlying `_search` function. Instead of using input parameters defined in the client, direct API query parameters are used instead. Documentation on which of these are available for each endpoint can be found [here](https://api.materialsproject.org/docs) or [here](https://api.materialsproject.org/redoc). While this information is provided, **most people can and should stick to using the standard `search` function.**

The code below is an example illustrating the difference in querying for data from the summary endpoint with volume criteria using `search` and `_search`:

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    # -- Using 'search'
    docs = mpr.materials.summary.search(volume=(3,10))
    
    # -- Using '_search'
    docs = mpr.materials.summary._search(volume_min=3, volume_max=10)
```
