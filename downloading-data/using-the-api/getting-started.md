---
description: Setting up the Materials Project API client.
---

# Getting Started

The **MPRester** is a Python client provided by the Materials Project for easily accessing data through its API. It can be found in the `mp-api` package, which can be installed most easily using `pip`:

```bash
pip install mp_api
```

As an alternative, the package can also be installed from [its code repository](https://github.com/materialsproject/api):

```bash
git clone https://github.com/materialsproject/api
cd api
pip install -e .
```

**An API key is needed in order to use the client.** This is a unique key provided to each Materials Project account. Your API key can be found on your [profile dashboard page](https://next-gen.materialsproject.org/dashboard) once logged in.

The **MPRester** client can then be imported and instantiated. It is preferred to use Python's `with` context manager for session management:

```python
from mp_api.client import MPRester

# Option 1: Pass your API key directly as an argument.
with MPRester("your_api_key_here") as mpr:
    # do stuff with mpr...

# Option 2: Use the `MP_API_KEY` environment variable:
# export MP_API_KEY="your_api_key_here"
# Note: You can also configure your API key through pymatgen
with MPRester() as mpr:
    # do stuff with mpr ...
```

See the following sections for details on how to query different types of data. Documentation for `MPRester`'s classes and methods can be found [here](https://materialsproject.github.io/api/). The table below lists the available endpoints, their document models, and purpose.

| Endpoint                                        | Document Model             | Purpose                                                                                                                                                                                     |
| ----------------------------------------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/doi`                                          | DOIDoc                     | DOIs to reference specific materials on Materials Project.                                                                                                                                  |
| `/materials/absorption`                         | AbsorptionDoc              | Absorption spectrum based on frequency dependent dielectric function calculations.                                                                                                          |
| `/materials/alloys`                             | AlloyPairDoc               | Not yet documented.                                                                                                                                                                         |
| `/materials/bonds`                              | BondingDoc                 | Structure graphs representing chemical bonds calculated from structure using near neighbor strategies as defined in pymatgen.                                                               |
| `/materials/chemenv`                            | ChemEnvDoc                 | Coordination environments based on cation-anion bonds computed for all unique cations in this structure. If no oxidation states are available, all bonds will be considered as a fall-back. |
| `/materials/core`                               | MaterialsDoc               | Not yet documented.                                                                                                                                                                         |
| `/materials/dielectric`                         | DielectricDoc              | A dielectric property block                                                                                                                                                                 |
| `/materials/elasticity`                         | ElasticityDoc              | Not yet documented.                                                                                                                                                                         |
| `/materials/electronic_structure/bandstructure` | ElectronicStructureDoc     | Definition for a core Electronic Structure Document                                                                                                                                         |
| `/materials/electronic_structure/dos`           | ElectronicStructureDoc     | Definition for a core Electronic Structure Document                                                                                                                                         |
| `/materials/electronic_structure`               | ElectronicStructureDoc     | Definition for a core Electronic Structure Document                                                                                                                                         |
| `/materials/eos`                                | EOSDoc                     | Fitted equations of state and energies and volumes used for fits.                                                                                                                           |
| `/materials/grain_boundaries`                   | GrainBoundaryDoc           | Grain boundary energies, work of separation...                                                                                                                                              |
| `/materials/insertion_electrodes`               | InsertionElectrodeDoc      | Insertion electrode                                                                                                                                                                         |
| `/materials/magnetism`                          | MagnetismDoc               | Magnetic data obtain from the calculated structure                                                                                                                                          |
| `/materials/oxidation_states`                   | OxidationStateDoc          | Oxidation states computed from the structure                                                                                                                                                |
| `/materials/phonon`                             | PhononBSDOSDoc             | Phonon band structures and density of states data.                                                                                                                                          |
| `/materials/piezoelectric`                      | PiezoelectricDoc           | A dielectric package block                                                                                                                                                                  |
| `/materials/provenance`                         | ProvenanceDoc              | A provenance property block                                                                                                                                                                 |
| `/materials/robocrys`                           | RobocrystallogapherDoc     | This document contains the descriptive data from robocrystallographer for a material: Structural features, mineral prototypes, dimensionality, ...                                          |
| `/materials/similarity`                         | SimilarityDoc              | Model for a document containing structure similarity data                                                                                                                                   |
| `/materials/substrates`                         | SubstratesDoc              | Possible growth substrates for a given material.                                                                                                                                            |
| `/materials/summary`                            | SummaryDoc                 | Summary information about materials and their properties, useful for materials screening studies and searching.                                                                             |
| `/materials/surface_properties`                 | SurfacePropDoc             | Model for a document containing surface properties data                                                                                                                                     |
| `/materials/synthesis`                          | SynthesisSearchResultModel | Model for a document containing synthesis recipes data and additional keyword search results                                                                                                |
| `/materials/tasks`                              | TaskDoc                    | Calculation-level details about VASP calculations that power Materials Project.                                                                                                             |
| `/materials/thermo`                             | ThermoDoc                  | A thermo entry document                                                                                                                                                                     |
| `/materials/xas`                                | XASDoc                     | Document describing a XAS Spectrum.                                                                                                                                                         |
| `/molecules/assoc`                              | MoleculeDoc                | Not yet documented.                                                                                                                                                                         |
| `/molecules/bonding`                            | MoleculeBondingDoc         | Representation of molecular bonding.                                                                                                                                                        |
| `/molecules/core`                               | MoleculeDoc                | Not yet documented.                                                                                                                                                                         |
| `/molecules/jcesr`                              | MoleculesDoc               | Molecules relevant to battery electrolytes.                                                                                                                                                 |
| `/molecules/orbitals`                           | OrbitalDoc                 | Not yet documented.                                                                                                                                                                         |
| `/molecules/partial_charges`                    | PartialChargesDoc          | Atomic partial charges of a molecule                                                                                                                                                        |
| `/molecules/partial_spins`                      | PartialSpinsDoc            | Atomic partial charges of a molecule                                                                                                                                                        |
| `/molecules/redox`                              | RedoxDoc                   | Molecular properties related to reduction and oxidation, including vertical ionization energies and electron affinities, as well as reduction and oxidation potentials                      |
| `/molecules/summary`                            | MoleculeSummaryDoc         | Summary information about molecules and their properties, useful for searching.                                                                                                             |
| `/molecules/tasks`                              | TaskDocument               | Definition of a Q-Chem task document                                                                                                                                                        |
| `/molecules/thermo`                             | MoleculeThermoDoc          | Not yet documented.                                                                                                                                                                         |
| `/molecules/vibrations`                         | VibrationDoc               | Not yet documented.                                                                                                                                                                         |
