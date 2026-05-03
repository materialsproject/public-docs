# Identifier systems used in the Materials Project

The Materials Project (MP) has long used material or `MPID`s of the form `mp-<integer>` to label both unique materials and unique calculations (single runs of a simulation).
As MP's set of calculations continually grows, these identifiers continue to grow in length, limiting their memorizability.
Additionally, the lack of distinction between the material labelled as `material_id = mp-149` and the calculation labelled as `task_id = mp-149` can be confusing for users of MP's data.

In addition to ease of recall, MP's identifiers are also sortable.
This has long been important for our builders, which process raw calculations/tasks.
Tasks are grouped by structures which are similar enough to identified as the same material (e.g., diamond-cubic Si with lattice constant 5 Å and 5.1 Å would be matched, but diamond-cubic Si and simple-cubic Si would not).
The MPID of a <emph>material</emph> is then taken as the lowest-valued task which is in that group.
Hence, `mp-149` is labelled by task `mp-149`, but task `mp-149` currently contributes no data to material `mp-149` - it has long been superseded by updated calculations.

#### Alphabetical identifiers

To derive a set of sortable, more memorable, and easily extensible identifiers, we have begun transitioning our IDs internally to alphabetical IDs.
This system uses base-26 math (based on the Latin lowercase alphabet) to represent integer-valued IDs.

In base-10 math, we represent numbers as coefficients multiplying $10^n$:
$$
2026 = 2 \times 10^3 + 0 \times 10^2 + 2 \times 10^1 + 6 \times 10^0
$$
In base-26 math, where $a=0$, $b=1$, $c=2$,..., $z=25$, the same number would be represented as:
$$
2026 = czy = 2 \times 26^2 + 25 \times 26^1 + 24 \times 26^0
$$
We call these new identifiers `AlphaID`.

<b>To ensure backwards compatibility, all <emph>materials or tasks</emph> with an identifier of `mp-3347529` or lower will be represented as `MPID`s: `MPID(mp-3347529)`.</b>

All newer calculations, beginning with those rolled out in [database version 2026-04-13](https://docs.materialsproject.org/changes/database-versions#v2026.04.13), will be labelled by default as `AlphaID`s.
To help distinguish calculations from built/processed materials data, tasks will be labelled without a prefix, e.g., `hilze` rather than `mp-hilze`.

Because `AlphaID`s are just a more compact representation of base-10 `MPID`s, users will be able to query for materials/tasks using either representation.

#### Global string sorting

Finally, to ease sorting of IDs, all IDs will be left-padded with zeros to ensure consistent string-sorting behavior.
In our same example as above, `aaczy` has the same value as `czy`, because this simply adds $0 \times 26^4 + 0 \times 26^3 = 0$.
To allow for a significant new block of calculations, <b>we are currently padding all IDs up to character length of 8</b>, however this may need to be increased with time.
Thus within the database, `MPID(mp-2026)` would be represented as `AlphaID(mp-aaaaaczy)`.
Now, without defining custom sort functions, this `AlphaID` will correctly be sorted after `MPID(mp-202) = AlphaID(mp-aaaaaahu)`, whereas a string sort would place `mp-202` before `mp-2026`.