---
description: Setting up the Materials Project API client.
---

# Getting Started

The **MPRester** is a Python client provided by the Materials Project for easily accessing data through its API. It can be found in the `mp-api` package, which can be installed most easily using `pip`:

```bash
pip install mp_api
```

As an alternative, the package can also be installed from [its code repository](https://github.com/materialsproject/api):&#x20;

```bash
git clone https://github.com/materialsproject/api
cd api
pip install -e .
```

**An API key is needed in order to use the client.** This is a unique key provided to each Materials Project account. Your API key can be found on your [profile dashboard page](https://next-gen.materialsproject.org/dashboard) or the main [API page](https://next-gen.materialsproject.org/api) once logged in.

The **MPRester** client can then be imported and instantiated. It is preferred to use Python's `with` context manager for session management:

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
    #do stuff with mpr...
```

See the following sections for details on how to query different types of data.

{% tabs %}
{% tab title="Relevant Code Links" %}
{% embed url="https://github.com/materialsproject/api/blob/main/mp_api/client/client.py#L41-L978" %}
MPRester client code
{% endembed %}
{% endtab %}
{% endtabs %}
