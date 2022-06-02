---
description: How is the symmetry determined?
---

# Symmetry

The symmetry of each MOF is determined using Pymatgen's [SpacegroupAnalyzer](https://pymatgen.org/pymatgen.symmetry.analyzer.html#pymatgen.symmetry.analyzer.SpacegroupAnalyzer) with a `symprec` tolerance of 0.1. The symmetry is based on the PBE-D3(BJ) optimized structure. It should be noted that all structure relaxations were carried out without explicit symmetry constraints of any kind.
