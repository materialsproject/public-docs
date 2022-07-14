---
description: How were pore-based properties computed?
---

# Pore Geometry

Pore-related properties were computed using [Zeo++](http://zeoplusplus.org) 0.3 with the high-accuracy flag (except in the rare cases where this failed, in which case the standard accuracy was used). These properties were computed using the PBE-D3(BJ) optimized structure.

The pore-limiting diameter is the smallest spherical diameter of void space that a guest species would need to traverse in order to diffuse through the material, whereas the largest cavity diameter is the largest spherical diameter that can fit within the void space of the material.
