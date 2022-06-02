# Frequently Asked Questions

## Where do the material properties shown on Materials Project come from?

The Materials Project core data is all calculated in-house by the Materials Project team using a variety of simulation methods. To understand the quality of these predictions, it is crucial to read the peer-reviewed publications from the Materials Project where each property is benchmarked as much as possible against known experimental values: this will give an estimate of typical error and, importantly, any systematic error that may be present.

## Why are the lattice parameters different to what I expect?

The same crystal structure can have multiple, equivalent sets of lattice parameters depending on what crystallographic "setting" is used.

Typically, there are two sets of lattice parameters reported. Lattice parameters can be defined for the **primitive** **cell**, which is a definition of the crystal with the fewest number of atoms and therefore conenient for simulations and other uses, and the **conventional cell**, which is typically easier to visualize and more like you will see in textbooks.

If the lattice parameters are very different to what you expect, check the setting first!

Some systematic errors are also present. These will typically be an over-estimation of 1–3% for most crystals. Layered crystals will also typically have significant error in the interlayer distances since van der Waals interactions are not described well by the simulation methods used by Materials Project. See [Calculation Details](methodology/calculation-details/) for more information.

## Why is the band gap different to what I expect?

Electronic band gaps are difficult to calculate reliably from first principles, especially using methods that scale well to hundreds of thousands of materials. The method used by the Materials Project (PBE) _systematically underestimates_ band gaps.

While it would be possible to provide higher quality calculations for a select number of materials, with more accurate band gaps, it is noted that for materials discovery purposes it is useful to have a dataset that has the same systematic error. See Electronic Structure for more information.

## Why has a value changed on Materials Project?

The Materials Project presents the data it generates in two ways:

1. As individual calculations. These are always the same, and as far as possible Materials Project tries to ensure all historical calculations remain available. Typically, only advanced users will access information about individual calculations.
2. As aggregated information. This is information generated from a combination of individual calculations. This information is what is presented on the public "material details" pages, and is what most users will access. As new, improved calculations are performed, this aggregated information can change.

The Materials Project periodically updates this aggregated information in the form of new database releases. See [Database Versions](database-versions.md) for information on the latest database releases.

{% hint style="warning" %}
If performing scientific research with Materials Project data, make sure to cite the database version from which the data was retrieved. See [How to Cite](https://materialsproject.org/about/cite) for more information.&#x20;
{% endhint %}
