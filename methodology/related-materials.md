---
description: 'TODO: Update references'
---

# Related Materials

## Introduction

The similarity between two structures _i_ and _j_ is assessed on the basis of local coordination information from all sites in the two structures. [\[1,2\]](related-materials.md#references) The four basic steps involved are:

1. Find near(est) neighbors of all sites in both structures.
2. Evaluate each coordination pattern via coordination descriptors observed at each site to define site fingerprints.
3. Compute statistics of the descriptor values across all sites in a structure to define structure fingerprints.
4. Use structure fingerprints to rate the (dis)similarity between the two (vectors representing the two) structures.

## Near-neighbor finding

We use a novel method called CrystalNN to find near(est) neighbors in periodic structures. While the method will be introduced shortly [\[3\]](related-materials.md#references), it is already available through the python package [pymatgen](https://github.com/materialsproject/pymatgen).

## Site Fingerprints

The second step of the structure similarity calculation is the computation of a crystal site fingerprint, $$v^{site}$$, for each site in the two structures. The fingerprint is a 61-dimensional vector in which each element carries information about the local coordination environment computed with the _site_ module of the python package [matminer](https://github.com/hackingmaterials/matminer). For example, the first two elements "wt $$\text{CN}_1$$" and "single bond $$\text{CN}_1$$" provide estimates of the likelihood (or weight) of how much the given site should be considered 1-fold coordinated (i.e., _w_$$|_{CN=1}$$_)._ The third element "wt $$\text{CN}_2$$" provides a 2-fold coordination likelihood, whereas the fourth element "L-shaped $$\text{CN}_2$$" holds the resemblance similarity to an L-shaped coordination geometry (also called local structure order parameter) given that we find a coordination configuration with 2 atoms ($$q_{L}|_{CN=2}$$). The local structure order parameters can assume values between 0, meaning that the observed local environment has no resemblance with the target motif to which it is compared, and 1, which stands for perfect motif match. The remaining elements are: "water-like $$\text{CN}_2$$", "bent 120 degrees $$\text{CN}_2$$", "bent 150 degrees $$\text{CN}_2$$", "linear $$\text{CN}_2$$", "wt $$\text{CN}_3$$", "trigonal planar $$\text{CN}_3$$", "trigonal non-coplanar $$\text{CN}_3$$", "T-shaped $$\text{CN}_3$$", "wt $$\text{CN}_4$$", "square co-planar $$\text{CN}_4$$", "tetrahedral $$\text{CN}_4$$", "rectangular see-saw-like $$\text{CN}_4$$", "see-saw-like $$\text{CN}_4$$", "trigonal pyramidal $$\text{CN}_4$$", "wt $$\text{CN}_5$$", "pentagonal planar $$\text{CN}_5$$", "square pyramidal $$\text{CN}_5$$", "trigonal bipyramidal $$\text{CN}_5$$", "wt $$\text{CN}_6$$", "hexagonal planar $$\text{CN}_6$$", "octahedral $$\text{CN}_6$$", "pentagonal pyramidal $$\text{CN}_6$$", "wt $$\text{CN}_7$$" "hexagonal pyramidal $$\text{CN}_7$$", "pentagonal bipyramidal $$\text{CN}_7$$", "wt $$\text{CN}_8$$" "body-centered cubic $$\text{CN}_8$$", "hexagonal bipyramidal $$\text{CN}_8$$", "wt $$\text{CN}_9$$", "q2 $$\text{CN}_9$$", "q4 $$\text{CN}_9$$", "q6 $$\text{CN}_9$$", "wt $$\text{CN}_{10}$$", "q2 $$\text{CN}_{10}$$", "q4 $$\text{CN}_{10}$$", "q6 $$\text{CN}_{10}$$", "wt $$\text{CN}_{11}$$", "q2 $$\text{CN}_{11}$$", "q4 $$\text{CN}_{11}$$", "q6 $$\text{CN}_{11}$$", "wt $$\text{CN}_{12}$$", "cuboctahedral $$\text{CN}_{12}$$", "q2 $$\text{CN}_{12}$$", "q4 $$\text{CN}_{12}$$", "q6 $$\text{CN}_{12}$$", "wt $$\text{CN}_{13}$$", "wt $$\text{CN}_{14}$$", "wt $$\text{CN}_{15}$$", "wt $$\text{CN}_{16}$$", "wt $$\text{CN}_{17}$$", "wt $$\text{CN}_{18}$$", "wt $$\text{CN}_{19}$$", "wt $$\text{CN}_{20}$$", "wt $$\text{CN}_{21}$$", "wt $$\text{CN}_{22}$$" "wt $$\text{CN}_{23}$$" and "wt $$\text{CN}_{24}$$" Note that $$q_n$$ refers to Steinhardt bond orientational order parameter of order n. The resulting site fingerprint is thus defined as:

$$
\mathbf{v}^\text{site} = [w|_{\text{CN}=1}, \quad w|_{\text{CN}=2}, \quad q_\text{L}|_{\text{CN}=2}, \quad q_\text{water}|_{\text{CN}=2}, \quad \dots, \quad w|_{\text{CN}=24}]^\text{T}
$$

## Structure Fingerprints

The fingerprints from sites in a given structure are subsequently statistically processed to yield the minimum, maximum, mean, and standard deviation of each coordination information element," The resultant ordered vector defines a structure fingerprint, $v^{struct}$:

$$
\mathbf{v}^\text{struct} = [ \min(w|_{\text{CN}=1}), \quad \max(w|_{\text{CN}=1}), \quad \text{mean}(w|_{\text{CN}=1}), \quad \text{std}(w|_{\text{CN}=1}), \dots, \min(w|_{\text{CN}=24}), \quad \max(w|_{\text{CN}=24}), \quad \text{mean}(w|_{\text{CN}=24}), \quad \text{std}(w|_{\text{CN}=24}) ]^\text{T}
$$

## Structure Distance/Dissimilarity

Finally, structure similarity is determined by the distance, _d_, between two structure fingerprints $$v_{i}^{struct}$$ and$$v_{j}^{struct}$$:

$$
d = || \mathbf{v}_{i}^\text{struct} - \mathbf{v}_{j}^\text{struct} ||
$$

A small distance value indicates high similarity between two structures, whereas a large distance (>1) suggests that the structures are very dissimilar," The spinel example below gives an approximate threshold up to which **distance you can still consider two structures to be similar (0.9)**," Anything beyond 0.9 is most certainly not the same structure prototype.

## Examples

* Diamond ([mp-66](https://materialsproject.org/materials/mp-66/)) vs. $$\text{GaAs}$$ ([mp-2534](https://materialsproject.org/materials/mp-2534/)) $$\rightarrow$$ _d_ = 0
* Diamond ([mp-66](https://materialsproject.org/materials/mp-66/)) vs. rocksalt ([mp-22862](https://materiahttps/materialsproject.org/materials/mp-5827/lsproject.org/materials/mp-22862/)) $$\rightarrow$$ _d_ = 3.5724
* Diamond ([mp-66](https://materialsproject.org/materials/mp-66/)) vs. perfect $$\text{CaTiO}_3$$ perovskite ([mp-5827](https://materialsproject.org/materials/mp-5827/)) $$\rightarrow$$ _d_ = 3.5540
* Rocksalt ([mp-22862](https://materialsproject.org/materials/mp-22862/)) vs. perfect $$\text{CaTiO}_3$$ perovskite ([mp-5827](https://materialsproject.org/materials/mp-5827/)) $$\rightarrow$$ _d_ = 2.7417
* $$\text{Ca(CoS}_2\text{)}_2$$-spinel ([mvc-12728](https://materialsproject.org/materials/mvc-12728/)) vs. $$\text{Si(CdO}_2\text{)}_2$$-spinel ([mp-560842](https://materialsproject.org/materials/mp-560842/)) $$\rightarrow$$ _d_ = 0.8877

Below is a python code snippet that allows you to quickly reproduce above results," You will need to install [pymatgen](https://github.com/materialsproject/pymatgen) and [matminer](https://github.com/hackingmaterials/matminer) for this to work," Both are easily accessible via the [Python Package Index](https://pypi.python.org/pypi).

```python
import numpy as np
from mp_api import MPRester
from matminer.featurizers.site import CrystalNNFingerprint
from matminer.featurizers.structure import SiteStatsFingerprint

with MPRester() as mpr:

    # Get structures.
    diamond = mpr.get_structure_by_material_id("mp-66")
    gaas = mpr.get_structure_by_material_id("mp-2534")
    rocksalt = mpr.get_structure_by_material_id("mp-22862")
    perovskite = mpr.get_structure_by_material_id("mp-5827")
    spinel_caco2s4 = mpr.get_structure_by_material_id("mvc-12728")
    spinel_sicd2O4 = mpr.get_structure_by_material_id("mp-560842")

    # Calculate structure fingerprints.
    ssf = SiteStatsFingerprint(
        CrystalNNFingerprint.from_preset('ops', distance_cutoffs=None, x_diff_weight=0),
        stats=('mean', 'std_dev', 'minimum', 'maximum'))
    v_diamond = np.array(ssf.featurize(diamond))
    v_gaas = np.array(ssf.featurize(gaas))
    v_rocksalt = np.array(ssf.featurize(rocksalt))
    v_perovskite = np.array(ssf.featurize(perovskite))
    v_spinel_caco2s4 = np.array(ssf.featurize(spinel_caco2s4))
    v_spinel_sicd2O4 = np.array(ssf.featurize(spinel_sicd2O4))

    # Print out distance between structures.
    print('Distance between diamond and GaAs: {:.4f}'.format(np.linalg.norm(v_diamond - v_gaas)))
    print('Distance between diamond and rocksalt: {:.4f}'.format(np.linalg.norm(v_diamond - v_rocksalt)))
    print('Distance between diamond and perovskite: {:.4f}'.format(np.linalg.norm(v_diamond - v_perovskite)))
    print('Distance between rocksalt and perovskite: {:.4f}'.format(np.linalg.norm(v_rocksalt - v_perovskite)))
    print('Distance between Ca(CoS2)2-spinel and Si(CdO2)2-spinel: {:.4f}'.format(np.linalg.norm(v_spinel_caco2s4 - v_spinel_sicd2O4)))
```

## References

\[1]: N. E. R. Zimmermann, D. Winston, K. A. Persson, A. Jain, in preparation (2018)&#x20;

\[2]: 10.3389/fmats.2017.00034&#x20;

\[3]: H. Pan, J. Dagdelen, N. E. R. Zimmermann, A. Jain, in preparation (2018)

## Authors

Nils Zimmermann, Donny Winston, Handong Ling, Oxana Andriuc
