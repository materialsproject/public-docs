---
description: >-
  Description of the components of the phase diagram app, the intended
  functionality of each component, and a short review of the origin of the phase
  diagram construction and MP thermodynamic data.
---

# Background

The **Phase Diagram App** allows a user to create and visualize compositional phase diagrams for 1, 2, 3, and 4 element chemical systems using Materials Project data. It is also possible to create and visualize the corresponding chemical potential diagrams for 2 and 3 element systems. The user has access to some customization features, such as 1) changing the style of plot, 2) selecting data calculated with a certain DFT functional, 3) and using machine learning (ML)-estimated finite temperature data.

## Methodology

The methodology behind thermodynamic energy calculations, phase diagram construction, and chemical potential diagram construction has been extensively discussed in the Methodology section of the MP Docs. See the links below:

{% content-ref url="../../../methodology/materials-methodology/thermodynamic-stability/thermodynamic-stability.md" %}
[thermodynamic-stability.md](../../../methodology/materials-methodology/thermodynamic-stability/thermodynamic-stability.md)
{% endcontent-ref %}

{% content-ref url="../../../methodology/materials-methodology/thermodynamic-stability/phase-diagrams-pds.md" %}
[phase-diagrams-pds.md](../../../methodology/materials-methodology/thermodynamic-stability/phase-diagrams-pds.md)
{% endcontent-ref %}

{% content-ref url="../../../methodology/materials-methodology/thermodynamic-stability/chemical-potential-diagrams-cpds.md" %}
[chemical-potential-diagrams-cpds.md](../../../methodology/materials-methodology/thermodynamic-stability/chemical-potential-diagrams-cpds.md)
{% endcontent-ref %}

{% content-ref url="../../../methodology/materials-methodology/thermodynamic-stability/finite-temperature-estimation.md" %}
[finite-temperature-estimation.md](../../../methodology/materials-methodology/thermodynamic-stability/finite-temperature-estimation.md)
{% endcontent-ref %}

## App Components

### Search by chemical system

Phase diagrams are created by **chemical system** (i.e., a collection of elements). To create a phase diagram in the Phase Diagram App, first select a set of elements by typing them either as a single string separated by dashes, or by clicking the elements in the periodic table viewer (which will auto-populate the search box).

![Figure 1: Search by chemical system using the periodic table viewer](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 4.13.26 PM.png>)

{% hint style="warning" %}
**Warning**

Phase diagrams can only be plotted for chemical systems containing **1-4 elements**. It is still possible to create phase diagrams for 5 or more elements, but this feature is only currently available in **** _pymatgen_.
{% endhint %}

### Visualization Viewer

Once a chemical system has been selected (e.g., Li-Fe-O), you will an illustration of the compositional phase diagram for your system of interest load in the the viewer.  Within the viewer, you can switch to the chemical potential diagram to view the same phase equilibria but within chemical potential space (see [Methodology](background.md#chemical-potential-diagrams) for more information).

**1) Phase Diagram**

![](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 4.27.33 PM.png>)

**2) Chemical Potential Diagram**

![](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 4.33.40 PM.png>)

If you are

### Configure Visualization

The phase diagram viewer can be configured&#x20;

![](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 4.25.25 PM.png>)

### Advanced Options

![](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 4.38.07 PM.png>)

### Data Table

![](<../../../.gitbook/assets/Screen Shot 2022-07-14 at 4.37.51 PM.png>)

