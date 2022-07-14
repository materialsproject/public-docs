---
description: What are SMILES strings, a MOFid, and a MOFkey?
---

# SMILES, MOFid, and MOFkey

In prior work by [Bucior et al.](https://doi.org/10.1021/acs.cgd.9b01050), a pair of methods known as MOFid and MOFkey are described that can be used to assign a unique name for a given MOF. MOFid works by deconstructing a MOF into its node(s), linker(s), and topology. The nodes and linkers are represented as SMILES strings, the topology is determined using [Systre](http://gavrog.org), and any catenation is noted. These factors are combined into a single unique "MOFid". The MOFkey is simply a shorter, InChI-based hash of the MOFid. These methods are shown below for HKUST-1 (also known as Cu3(btc)2 and Cu-BTC):

![Example MOFid and MOFkey for the MOF with common name HKUST-1.](https://snurr-group.github.io/web-mofid/mofid.png)

The MOFid code is available [here](https://github.com/snurr-group/mofid) with a web-based version available [here](https://snurr-group.github.io/web-mofid).

The SMILES search on the MOF Explorer is a partial-match of the MOFid. As such, one can query by just the node, just the linker, or even a substructure of the linker. If the user wishes to supply both a node and linker query, they should be provided in the MOFid format (separated by a "." with the node(s) listed before the linker(s)).
