# Magnetic Properties

The magnetic behavior of a material is a complex and rich research area. The Materials Project currently only addresses a narrow aspect of the magnetism of materials: the magnitude and ordering of atomic magnetic moments in a crystal structure, at zero temperature.

At present, Materials Project only considers _**** collinear magnetic order_ which means that atomic magnetic moments are represented by scalar values and not vectors.

The Materials Project approaches magnetism in two ways:

1. Historically, _all_ materials are initialized in a _ferromagnetic_ configuration by default. This was a pragmatic choice due to the computational expensive of considering all possible magnetic ordering. During the simulation of these materials, it is possible that the magnetic order will converge to a non-ferromagnetic order, but more likely the order will remain ferromagnetic even if the true ground state of the material is non-ferromagnetic. Therefore, the reported magnetic order for most materials on the Materials Project is a _description_ of the calculated magnetic order, and _not a prediction_ of the true ground state magnetic order.
2. For some materials, Materials Project has started to systematically search for ground state magnetic ordering of materials. This means that multiple magnetic ground states are considered for each material: ferromagnetic, anti-ferromagnetic, ferrimagnetic, etc. So far, this systematic search has been done for several thousand magnetic oxides. _For these materials, the reported magnetic order is therefore a prediction of the true ground state magnetic order._

{% embed url="https://www.science.org/doi/full/10.1126/sciadv.abd1076" %}
Publication detailing the Materials Project strategy for magnetic materials.
{% endembed %}
