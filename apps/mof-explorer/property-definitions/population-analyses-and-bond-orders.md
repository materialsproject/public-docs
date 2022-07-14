---
description: >-
  How were partial atomic charges, atomic spin densities, and effective bond
  orders computed?
---

# Population Analyses and Bond Orders

All partial atomic charges are computed at the PBE-D3(BJ) geometry using one of several population analysis methods: [Bader](http://theory.cm.utexas.edu/henkelman/code/bader), [DDEC6](https://doi.org/10.1039/C6RA04656H), and [CM5](https://doi.org/10.1021/ct200866d). The DDEC6 and CM5 charges were calculated from [Chargemol 09-26-2017](https://sourceforge.net/projects/ddec/files). In all cases, the partial atomic charges are calculated from the DFT-computed charge density. In the QMOF Database, partial atomic charges are calculated using a charge density at one of four several levels of theory: PBE, HLE17, HSE06\*, and/or HSE06. In general, the different levels of theory predict similar partial atomic charges. The different charge partitioning schemes, however, can result in very different partial atomic charges.

We report multiple magnetic properties for each material, including a net magnetic moment, atomic magnetic moments from VASP, and atomic spin densities calculated using the [Bader](http://theory.cm.utexas.edu/henkelman/code/bader) and [DDEC6](https://doi.org/10.1039/C6RA04656H) methods. In general, a high-spin magnetic initialization was provided (similar to what is done for the Materials Project). We note, however, that this does not mean all materials have high-spin character, as the initial magnetic moments are adjusted until they converges to a local minimum energy configuration in VASP. For applications that are highly reliant on an accurate description of the magnetic character, we acknowledge that there may be a lower energy magnetic configuration not captured via this initialization procedure.

Bond orders were computed using the [DDEC6](https://doi.org/10.1039/C7RA07400J) method. Bond orders displayed in the MOF Explorer are the effective bond orders for each atom (i.e. a single value representing the sum of bond orders between the atom of interest and its neighbors). For the full list of bond orders between every pair of atoms, we refer the user to the DDEC6 files made available on NOMAD. For the crystal structure visualization, bond orders are not taken into account; instead, the coordination environments are determined from Pymatgen's [CrystalNN algorithm](https://pymatgen.org/pymatgen.analysis.local\_env.html#pymatgen.analysis.local\_env.CrystalNN).
