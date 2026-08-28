---
description: >-
  Add new data to a project by loading a file, reviewing it in an editable grid,
  and submitting it.
---

# Upload Tab

### Getting started

Select your project from the dropdown at the top of the page, then either drag and drop a file, click **select** to choose a CSV or Excel file (`.csv`, `.xls`, `.xlsx`), or paste a Google Sheet URL (the sheet must have "view with link" permissions enabled; this can be set from the Sheet's own **Share** menu). Choose whether your data should be loaded as `nested` or `flat`, then click `Load Data`.

> If your column headers use dot notation (for example, `band.gap`), **nested** mode treats the dots as a hierarchy separator, grouping related fields together. Choose the option that matches how your column names are structured.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 11.48.00 AM.png" alt=""><figcaption><p>Upload tab prior to loading data.</p></figcaption></figure>

### The submission checklist

Once your data is loaded, a checklist appears in the top right corner, tracking everything that needs to be resolved before you can submit. The items are: `no blank identifiers`, `no blank formulas`, `no in-batch duplicate identifiers`, `new columns` and `in-project duplicates`. Each of these is explained in detail below. The Submit button remains disabled until every item on the checklist has been resolved.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 11.51.50 AM.png" alt=""><figcaption><p>Upload tab after loading data, the red arrow highlights the submission checklist.</p></figcaption></figure>

### Required: `identifier` and `formula` columns

Every file you upload must contain a column named `identifier` and a column named `formula`. These are the unique key tying each row to a specific contribution (for example, a material ID) and the chemical formula that corresponds with the contribution.

> **Hard block.** If no `identifier` and/or `formula` column is found, the file cannot be loaded at all. You will see an error message telling you to add one before trying again.

> **Hard block.** Neither `identifier` nor `formula` can have a unit. If either column appears with a unit suffix (for example, `identifier (mol)`), the file is blocked from loading. You will see an error telling you to remove the unit and re-upload.

### Loading your data

Once loaded, your data appears in an editable grid, with a column for each data field and a row for each entry. Column headers are automatically normalized to camelCase, for example, `BAND GAP`,  `Band Gap`, and `band gap` all become `bandGap`. If any column names were changed during normalization, you will see a confirmation message in the bottom right listing which ones changed; otherwise, you will see a general confirmation that your data loaded successfully.

> **Hard block.** If two or more columns normalize to the same name (for example, "Band Gap" and "band\_gap" both becoming `bandGap`), the file is blocked from loading entirely. You will see an error message listing the conflicting column names; rename them in your file and re-upload.

Columns with empty or unnamed headers (for example, phantom columns Excel sometimes generates from stray formatting) are automatically removed during loading. If you notice a column missing from your grid that you expected to see, check that its header was not blank in the original file.

> **Hard block.** A column header can have at most one set of parentheses, used to specify a unit (for example, `Length (cm)`). Headers with more than one set of parentheses, or with nested parentheses, are blocked from loading. You will see an error message listing the affected column names; rename them and re-upload.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 1.38.16 PM.png" alt=""><figcaption><p>Upload tab after loading data, the red arrow highlights the column name normalization message.</p></figcaption></figure>

Blank values are allowed in most columns — only `identifier` and `formula` cannot contain empty cells.

### Duplicate and existing identifiers

If two or more rows in your upload share the same identifier, they are flagged automatically and shown with strikethrough styling in the grid. Blank identifiers are never counted as duplicates of one another. This is what the checklist's in-batch duplicate rule is checking for.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 1.42.44 PM.png" alt=""><figcaption><p>Upload tab after initial data submission. Identifiers with strikethrough (see red arrow) are in-project duplicates. The red message must be acknowledged by the user to complete the submission checklist.</p></figcaption></figure>

If an identifier you are uploading already exists in the target project, that row is also shown with strikethrough styling at load time. This is not a hard block, it corresponds to the checklist's `In-project duplicates` rule, and you will need to check an acknowledgment box confirming that submitting will overwrite that contribution's existing data before you can proceed.

If your upload introduces columns that do not yet exist in the project, this corresponds to the checklist's `New columns` rule, and you will similarly need to acknowledge a warning message that existing contributions will not have data for these new columns before you can proceed.

### Editing your data

Click into any cell in the grid to edit its value directly. Editing an identifier is validated as you type: if the new value collides with another row in your current batch, the edit is automatically reverted and you will see an error message explaining why. Editing an identifier to match one that already exists in the project is allowed, this is the mechanism behind the overwrite flow described above.

### Adding a new field

Click `Add Field` to define a new column. Give it a `Field Name`, an optional `Default Value`, and a `Field Type`. Choose `measurement` for numeric values that have a unit (for example, a unit like `eV`), or `description` for text or categorical values that do not have a unit. Click `Confirm Field` to add it to the grid.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 1.53.21 PM.png" alt=""><figcaption><p>Upload tab with the Add Field menu displayed and ready to be filled out.</p></figcaption></figure>

### Editing an existing field

Click on any field name in the **Data Field** list to view and edit its settings. If the field already exists in the project's schema, its unit and rename controls are locked, you can view the field's current settings but cannot change them from here. For new fields not yet in the project, you can:

* `Rename` the field
* Change its current `unit`
* `Reassign` the field to merge it into a different existing field, combining the two into one column rather than keeping them separate

> **Hard block.** The two mandatory columns,  `identifier` and  `formula`, are not allowed to be renamed along with any previously submitted columns.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 1.57.35 PM.png" alt=""><figcaption><p>Upload tab with the editing menu for a previously submitted column that is blocked from renaming. If this batch had not already been submitted, a user would be allowed to edit all of the fields shown in the image.</p></figcaption></figure>

### Adding a new entry

Click `Add Entry` to manually add a new row. Fill in the `Value` for each field, then click `Confirm Entry` to add it to the grid alongside your uploaded data.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 1.59.42 PM.png" alt=""><figcaption><p>Upload tab with the Add Entry menu open, user will fill in a value for each field. </p></figcaption></figure>

### Submitting

Once every item on the checklist described above has been resolved, the Submit button becomes active. Click **Submit Data** to proceed.

You will immediately see a message confirming your submission is underway; for large submissions this can take a few minutes. A progress indicator on the Submit button shows how far along the upload is for new contributions, updating in increments as they are processed. Rows that update existing contributions do not show intermediate progress.

Once the submission completes, you will see a message confirming your data was submitted successfully.

If any rows have a blank identifier or formula at the moment you click submit, those rows are moved to the top of the grid so you can find and fix them without scrolling.

### Common errors and what they mean

<table data-search="false"><thead><tr><th>Error</th><th>What it means</th><th>What to do</th></tr></thead><tbody><tr><td>"Cannot load: no 'identifier' column found"</td><td>Your file has no column named <code>identifier</code></td><td>Add an <code>identifier</code> column and re-upload</td></tr><tr><td>"Cannot load: no 'formula' column found"</td><td>Your file has no column named <code>formula</code></td><td>Add a <code>formula</code> column and re-upload</td></tr><tr><td>"Cannot load: the column(s) 'identifier'/'formula' cannot have a unit"</td><td>The <code>identifier</code> or <code>formula</code> column has a unit suffix</td><td>Remove the unit from the column header and re-upload</td></tr><tr><td>"Cannot load: column(s) ... Units must be at the end of the submitted column header"</td><td>A column header has more than one set of parentheses, or nested parentheses</td><td>Rename the column so it has at most one set of parentheses, reserved for a trailing unit</td></tr><tr><td>"Cannot load: the following column names are duplicates after normalization"</td><td>Two or more columns normalize to the same name</td><td>Rename the columns so they do not collide after normalization</td></tr><tr><td>"Cannot load: file contains no data rows after removing empty rows"</td><td>Every row in your file was empty</td><td>Check your file for actual data rows</td></tr><tr><td>"N row(s) is/are missing an identifier/identifiers and M row(s) is/are missing a formula/formulas and cannot be submitted"</td><td>One or more selected rows have no identifier and/or formula value</td><td>Fill in the missing value(s); the affected rows are moved to the top of the grid</td></tr><tr><td>"The following identifier(s) appear more than once in this batch and must be resolved before submitting: ..."</td><td>Two or more rows share the same identifier</td><td>Make each identifier unique before submitting</td></tr><tr><td>"Error: Identifier 'X' is already used by another row in this batch. Choose a different identifier; this edit was reverted."</td><td>You edited a cell to an identifier already used elsewhere in your current batch</td><td>Choose a different identifier; your edit has already been reverted</td></tr></tbody></table>

In-project duplicates are not represented by an error message. Instead, rows with an identifier that already exists in the project are shown with strikethrough styling at load time, and you must check an acknowledgment box before submitting. See Duplicate and existing identifiers above.
