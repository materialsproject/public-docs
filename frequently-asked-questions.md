# Frequently Asked Questions (FAQ)

This page contains answers to common questions about the Materials Project.

See also our "Glossary of Terms" page which defines common terms in use by Materials Project.

{% content-ref url="glossary-of-terms.md" %}
[glossary-of-terms.md](glossary-of-terms.md)
{% endcontent-ref %}

## How do I sign in to the Materials Project?

You can login to the Materials Project either using an existing social identity provider (currently GitHub, Google, Facebook, Microsoft or Amazon) or via an email link.

{% hint style="info" %}
Be aware, your Materials Project account is linked to both your email address and the method that you log in. If you log in via a different method, this will be registered as a new account.
{% endhint %}

Here are some issues people have encountered when trying to sign in the [Materials Project](https://materialsproject.org/) website, and their solutions:

*   **I want to log in with my social identity provider (GitHub/Google/Facebook/Microsoft/Amazon), but I can’t.**

    Ensure that your password for your provider is correct (go to their site and log in there), ensure that you have a full name set on that account, and ensure that you allow Materials Project to see your basic profile info (name and email address).

    You also may be behind a firewall that doesn’t allow GitHub/Google/Facebook/Microsoft/Amazon. In that case, use our email based option instead.
*   **I appear to sign in OK (the popup goes away), but then I remain on the sign-in screen.**

    It may take a few seconds, depending on your connection, to actually get logged in. This is because we have an external identity provider verify your email address so that we don’t have to store any passwords on our servers.

    You may also have an older browser that won’t work well with our website at all. The latest version of Mozilla Firefox, Google Chrome, or Microsoft Edge will work well. Older versions of Internet Explorer will not work.
*   **I tried using the email option several times but haven’t received a login** link.

    We currently don’t do any validation of your email addresses, so if it “looks right”, i.e. you mistype [_myname@gmail.com_](mailto:myname@gmail.com) as _myname@**gmali**.com_, we will still try to send to the wrong address. Also check your "Spam" or "Junk" folder in case the login email has been flagged.

    There is a known issue with Tencent @qq.com addresses, where Tencent throttles delivery and you might not get an email within a reasonable amount of time. Please consider using an alternative to your @qq.com address for login.

## How do I cite Materials Project?

Citations are appropriate wherever Materials Project data, methods or output are used. See this page on the Materials Project website for more information:

{% embed url="https://materialsproject.org/about/cite" %}
How to Cite Materials Project
{% endembed %}

There is a canonical Materials Project citation, and additional citations for specific properties or tools. See also the [Database Versions](database-versions.md) page for information on how to cite a specific database version.

## Where do the material properties shown on Materials Project come from?

The Materials Project core data is all calculated in-house by the Materials Project team using a variety of simulation methods. To understand the quality of these predictions, it is crucial to read the peer-reviewed publications from the Materials Project where each property is benchmarked as much as possible against known experimental values: this will give an estimate of typical error and, importantly, any systematic error that may be present.

## Why are the lattice parameters different to what I expect?

The same crystal structure can have multiple, equivalent sets of lattice parameters depending on what crystallographic "setting" is used.

Typically, there are two sets of lattice parameters reported. Lattice parameters can be defined for the **primitive** **cell**, which is a definition of the crystal with the fewest number of atoms and therefore convenient for simulations and other uses, and the **conventional cell**, which is typically easier to visualize and more like you will see in textbooks.

If the lattice parameters are very different to what you expect, check the setting first!

Some systematic errors are also present. These will typically be an over-estimation of 1–3% for most crystals. Layered crystals will also typically have significant error in the interlayer distances since van der Waals interactions are not well-described by the simulation methods (PBE) used by Materials Project. These systematic errors will be improved as Materials Project switches to user newer simulation methods (r2SCAN). See [Calculation Details](methodology/calculation-details/) for more information.

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

## What is a "task\_id" and what is a "material\_id" and how do they differ?

Every database needs a unique key which can be used to distinguish one entry from another. In the Materials Project, each unique material is given a `material_id` (also referred to in various places as mp-id, mpid, MPID). This allows a specific polymorph of a given material to be referenced. For example, wurtzite GaN is assigned the `material_id` of [`mp-804`](https://materialsproject.org/materials/mp-804), while zinc blende GaN is assigned a `material_id` of [`mp-830`](https://materialsproject.org/materials/mp-830).

### How does a "material\_id" get assigned?

The Materials Project is a computational resource. All of the information on a given _material details page_ is actually a combination of data generated from many individual calculations or "tasks". It is also important that these tasks also have unique identifiers.

When a task is added to the Materials Project database, it will get an identifier assigned with the format `mp-[0-9]` ("mp-" with numbers after it). These identifiers are assigned sequentially, so smaller numbers usually refer to older calculations. An identifier referring to an _individual calculation task_ are known as a `task_id`.

When the Materials Project database is built, a unique material will then have a collection of multiple different task\_ids associated with it. The numerically smallest `task_id` will then become the `material_id`. This ensures that, as new, additional calculations are associated with the same material, its `material_id` should not change.

### In the past, I have seen material\_ids that start with "mvc", what are these?

Some calculation tasks were associated with a search for multivalent cathode materials. These tasks were given the prefix `mvc-` instead of `mp-` and thus some materials also had the prefix `mvc-`. However, this caused confusion and this approach has been retired. Tasks with the prefix `mvc-` still exist since the `task_id` cannot change, but a `material_id` will now always start with an `mp-` prefix by convention provided that at least one task associated with that material has the `mp-` prefix.

### Do material\_ids ever change? Do task\_ids ever change?

A `task_id` will never change. It will always refer to the same, individual calculation task.

A `material_id` might change in rare instances, such as the removal of the `mvc-` prefix, although this is avoided wherever possible.

If a `material_id` _does_ change, we ensure a redirect on the website is always in place, and the new `material_id` can also be found programmatically with the API using the `get_material_id_from_task_id()` function. This way, any publications or research that reference an older `material_id` are still valid, and the relevant data can still be retrieved.

****

## What does \_\_\_\_\_\_ mean?

Consult our glossary here:

{% content-ref url="glossary-of-terms.md" %}
[glossary-of-terms.md](glossary-of-terms.md)
{% endcontent-ref %}

If a term is used in Materials Project but is not listed, [let us know](getting-help.md) and we will add it.
