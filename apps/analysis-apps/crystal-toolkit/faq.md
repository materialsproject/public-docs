# FAQ

## What input file formats are supported?

Any input file format supported by [pymatgen](https://pymatgen.org). This includes CIF, POSCAR, CSSR and pymatgen JSON. See [here](https://pymatgen.org/pymatgen.core.structure.html#pymatgen.core.structure.IStructure.from\_file) for more information. Additional file formats might be supported on request.

## A transformation doesn't seem to be working?

{% hint style="danger" %}
This is a known issue in some instances, where transformations may time out for certain larger crystal structures or combinations of inputs. Any transformation taking longer than 30 seconds will time out.
{% endhint %}

Please ask on the [Materials Project forum](../../../getting-help.md) if this is a problem for you. We are improving this functionality over time. For advanced users, all transformations are also available for use via [pymatgen](https://pymatgen.org).
