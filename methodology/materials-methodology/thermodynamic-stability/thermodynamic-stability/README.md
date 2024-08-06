---
description: >-
  How energy adjustments and corrections are calculated on the Materials Project
  (MP) website.
---

# Energy Corrections

To better model energies across diverse chemical spaces, we apply several adjustments to the total calculated energy of each material. These adjustments fall into two different sets, each of which is described in a different subsection. One set, consisting of anion and GGA/GGA+U mixing scheme corrections, and another consisting of only GGA/GGA+U/r2SCAN mixing scheme corrections. The former is used in the in the current and legacy data, while the latter is only present in releases after the addition of r2SCAN calculations (post `v2022.10.28`). Both of them are used to produce `ComputedStructureEntry` objects, and mixed phase diagrams.



{% content-ref url="anion-and-gga-gga+u-mixing.md" %}
[anion-and-gga-gga+u-mixing.md](anion-and-gga-gga+u-mixing.md)
{% endcontent-ref %}



##
