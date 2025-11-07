---
description: How grain boundaries are calculated on the Materials Project (MP) website.
---

# Grain Boundaries

Many real, bulk crystalline materials consist of many grains separated by grain boundaries (GBs). GBs are interfaces where crystals of the same phase meet with different orientations. These boundaries strongly affect mechanical, electrical, and chemical properties, making them critical in materials design.

On MP, the GB structures and properties can be accessed through the API or generated via the Crystal Toolkit. The toolkit employs the Coincidence Site Lattice (CSL) method, where two misoriented crystals are superimposed and coincident lattice sites identified. The algorithm (implemented in pymatgen) constructs the GB structure by:<sup>1</sup>

1\.     **Lattice Transformation** – transform the unit cell into a CLS unit cell where the _a_ and _b_ vectors lie parallel to the desired GB plane.

2\.     **Grain Creation and Rotation** – build two grains and rotate them relative to each other using the specified axis and angle.

3\.     **Grain Stacking** – place the two grains together, adjusting relative shifts along the _a_, _b_, and _c_ directions.

4\.     **Atom Merging** – remove atoms that are too close, based on a user-defined distance tolerance.

<figure><img src="../.gitbook/assets/Screenshot 2025-08-18 at 2.44.20 PM.png" alt=""><figcaption></figcaption></figure>

```python
from mp_api.client import MPRester

with MPRester("your_api_key_here") as mpr:
   grain_bdy = mpr.materials.grain_boundaries.search()
```

## References

\[1] Zheng, H. _et al._ Grain boundary properties of elemental metals. _Acta Mater._ vol. 186, pp. 40–49, yr. 2020. ([DOI](https://doi.org/10.1016/j.actamat.2019.12.030))

## Authors

Mona Abdelgaid
