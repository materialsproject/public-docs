---
description: >-
  Background, tutorials, and FAQ for the Crystal Toolkit app on the Materials
  Project (MP) website.
---

# Crystal Toolkit

"Crystal Toolkit" refers to two things:

* An app on the Materials Project that allows manipulation and transformation of crystal structures, both from the Materials Project database or uploaded by the user.
* It refers to the [underlying Crystal Toolkit code](https://docs.crystaltoolkit.org), which was developed to write this app and which now powers the entire Materials Project website!

This documentation page shows how to use the app specifically.

## [Example of how to use the Crystal Toolkit app to export a crystal slab (surface)](https://app.tango.us/app/workflow/80c31346-a846-4e97-af5a-14ff05168d1d?utm\_source=markdown\&utm\_medium=markdown\&utm\_campaign=workflow%20export%20links)

#### 1. [Go to Materials Project - Crystal Toolkit](https://materialsproject.org/toolkit)

#### 2. Enter the Materials ID for the crystal structure you're interested in

For example, "mp-5229" which will load the structure shown in https://materialsproject.org/materials/mp-5229 ![Step 2 screenshot](https://images.tango.us/public/screenshot\_b2d44451-e4cc-4977-827a-bbfe1851273d.png?crop=focalpoint\&fit=crop\&fp-x=0.7101\&fp-y=0.4912\&fp-z=2.6652\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)

#### 3. Click on Search

This loads the crystal structure into the app. ![Step 3 screenshot](https://images.tango.us/public/screenshot\_57ccca19-185a-4c55-b35f-286c5bb1f248.png?crop=focalpoint\&fit=crop\&fp-x=0.8385\&fp-y=0.4912\&fp-z=2.8841\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)

#### 4. Use the drop down to list available transformations

Not all transformations will be applicable to all materials. ![Step 4 screenshot](https://images.tango.us/public/screenshot\_645e1d8d-fad3-4be4-91dc-95b30d00502a.png?crop=focalpoint\&fit=crop\&fp-x=0.3534\&fp-y=0.5035\&fp-z=1.2975\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)

#### 5. Select "Make a slab"

![Step 5 screenshot](https://images.tango.us/public/screenshot\_c103b4ce-dc43-4f32-bc66-8f90e8059542.png?crop=focalpoint\&fit=crop\&fp-x=0.3531\&fp-y=0.6268\&fp-z=1.2982\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)

#### 6. Change options for this transformation

Options directly map to those available in pymatgen. For this example, this maps to `SlabTransformation` [https://pymatgen.org/pymatgen.transformations.advanced\_transformations.html#pymatgen.transformations.advanced\_transformations.SlabTransformation](https://pymatgen.org/pymatgen.transformations.advanced\_transformations.html#pymatgen.transformations.advanced\_transformations.SlabTransformation)

All options have help available by hovering over the label. ![Step 6 screenshot](https://images.tango.us/public/screenshot\_050ccf93-5a30-403b-af80-f76c555f2a1d.png?crop=focalpoint\&fit=crop\&fp-x=0.1715\&fp-y=0.5093\&fp-z=2.7180\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)

#### 7. Toggle the switch next to the transformation to active the transformation

Applying the transformation might take several seconds. ![Step 7 screenshot](https://images.tango.us/public/screenshot\_93e0a9e2-6588-4cb9-8c5e-52b1c952cf6f.png?crop=focalpoint\&fit=crop\&fp-x=0.1520\&fp-y=0.2307\&fp-z=2.9568\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)

#### 8. Rotate the crystal structure to see the transformation applied

![Step 8 screenshot](https://images.tango.us/public/screenshot\_31ec8bc2-85e2-483a-9978-99ab843dc989.png?crop=focalpoint\&fit=crop\&fp-x=0.3531\&fp-y=0.2576\&fp-z=1.1077\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)

#### 9. Crystal structures can be easily exported to one of several output formats.

![Step 9 screenshot](https://images.tango.us/public/screenshot\_d61a1bb6-f3cf-4171-a506-116ef678a40e.png?crop=focalpoint\&fit=crop\&fp-x=0.5514\&fp-y=0.3172\&fp-z=2.8841\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)

#### 10. For example, the POSCAR format if you want to simulate this surface using the VASP simulation code.

![Step 10 screenshot](https://images.tango.us/public/screenshot\_c6230f19-6594-4818-9572-a2e7c964c837.png?crop=focalpoint\&fit=crop\&fp-x=0.4911\&fp-y=0.4509\&fp-z=2.1599\&w=1200\&mark-w=0.2\&mark-pad=0\&mark64=aHR0cHM6Ly9pbWFnZXMudGFuZ28udXMvc3RhdGljL21hZGUtd2l0aC10YW5nby13YXRlcm1hcmsucG5n\&ar=2356%3A1712)
