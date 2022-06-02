---
description: How do I find a MOF by its common name?
---

# Finding MOFs by Common Name

Frequently, one is interested in identifying a MOF with a specific common name (e.g. HKUST-1, MOF-5, MOF-74). While common names cannot directly be queried in the MOF Explorer, MOFid/MOFkey can be used to carry out such a query using the following general procedure:

1. Download the CIF of the desired MOF from the published literature (e.g. from the original source publication). Some common MOFs can be found [here](https://github.com/iRASPA/RASPA2/tree/master/structures/mofs/cif).
2. Calculate the MOF's unique MOFid or MOFkey using the web-based [ID Tool](https://snurr-group.github.io/web-mofid/) by simply uploading the structure and clicking submit. Please read the tips on the MOFid webpage carefully.
3. Copy down the MOFid and/or MOFkey information.
4. Query the MOF Explorer by SMILES (i.e. MOFid) or MOFkey. If there are multiple options, take the one you like. If multiple entries are returned in the MOF Explorer with the same reduced chemical formula, we generally recommend the structure with the lowest energy (per atom). This would represent the lowest energy conformer at the PBE-D3(BJ) level of theory.

There are other, slightly less comprehensive, ways of searching for a given MOF. For instance, you can search by DOI on the MOF Explorer, so if you know the DOI of the paper that reported the crystal structure of your MOF of interest, you can query by that. Additionally, if you know the CSD Refcode for a given MOF, you can query by that as well.
