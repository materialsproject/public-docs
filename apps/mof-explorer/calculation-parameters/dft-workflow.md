---
description: Running a QMOF-compatible workflow
---

# DFT Workflow

If you wish to run a QMOF-compatible workflow, we currently recommend using [QuAcc](https://github.com/arosen93/quacc), which has a QMOF "recipe" available at `from quacc.recipes.vasp.qmof import QMOFRelaxJob`. The QMOF workflow can be run via the following code-block:

```
from ase.io import read
from jobflow.managers.local import run_locally

from quacc.recipes.vasp.qmof import QMOFRelaxJob

# Read a MOF CIF
mof = read("mymof.cif")

# Make a QMOF-compatible job with on-the-fly error handling
job = QMOFRelaxJob().make(mof)

# Run the job locally, with all output data stored in a convenient schema
response = run_locally(job, create_folders=True)
```

