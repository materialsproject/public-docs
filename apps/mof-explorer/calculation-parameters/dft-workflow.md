---
description: >-
  How to run a density functional theory (DFT) workflow for calculating /
  optimizing MOFs.
---

# DFT Workflow

If you wish to run a QMOF-compatible workflow, we currently recommend using [QuAcc](https://github.com/arosen93/quacc), which has a QMOF "recipe" available at `from quacc.recipes.vasp.qmof`.

First, install QuAcc via `pip install quacc[vasp]`. The QMOF workflow can be run via the following code-block after the setup process is completed:

```python
import covalent as ct
from ase.io import read
from quacc.recipes.vasp.qmof import qmof_relax_job

# Read a MOF CIF
mof = read("mymof.cif")

# Make a QMOF-compatible job with on-the-fly error handling
workflow = ct.lattice(qmof_relax_job)

# Dispatch the workflow to the Covalent server
# with the Atoms object as the input
dispatch_id = ct.dispatch(workflow)(mof)

# Fetch the result from the server, if present
result = ct.get_result(dispatch_id)
print(result)
```

