---
description: >-
  Edit, delete, and update data that is already in a project, directly from the
  browser, no re-upload required.
---

# Edit Tab

### Getting started

Your own project will show up in the dropdown at the top of the page. Select the project you want to edit data for.

Once loaded, all contributions in the selected project appear in the grid. If the project has no contributions yet, the grid will appear empty.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 2.03.45 PM.png" alt=""><figcaption><p>Edit tab after loading the selected project's contributions.</p></figcaption></figure>

### The submission checklist

Similarly to the upload tab, as you make changes, a checklist tracks whether your data is ready to submit. The Edit tab checklist has two items: `no blank identifiers`, and `no blank formulas`. The Submit button remains disabled until both pass.

### Editing your data

Click into any cell in the grid to edit its value directly.

Editing an identifier is validated as you type. If the new value collides with another row currently in the grid (which encompasses the entire project), the edit is automatically reverted.&#x20;

If a row is staged for deletion (see below), its identifier is excluded from collision checks, so you can reuse that identifier on another row in the same session.

Non-identifier fields can be edited freely with no additional validation.

### Deleting rows

Select one or more rows using the checkboxes in the grid, then click **Delete Selected Rows**. You will be asked to confirm:

> Remove _n_ selected row(s) from this project? Be sure to submit all changes.

<figure><img src="../../.gitbook/assets/Screenshot 2026-08-25 at 2.06.15 PM.png" alt=""><figcaption><p>Edit tab row deletion confirmation dialogue box.</p></figcaption></figure>

Confirming removes the rows from the grid immediately, but nothing is sent to the project until you click Submit Changes.

### Deleting columns

Select one or more columns from the column selector dropdown, then click **Delete Selected Columns**. The `identifier` and `formula` columns cannot be selected for deletion. Clicking Delete shows a confirmation:

> Remove column(s): _column names_? Be sure to submit all changes.

Confirming hides the selected columns from the grid, but nothing is sent to the project until you click Submit.

If deleting a column would leave one or more contributions with no data at all, submission is blocked until those contributions are addressed. You will see a message listing the affected contributions and asking you to delete those rows first, then delete the column.

### Submitting

Once the checklist is resolved, click **Submit Changes**. You will immediately see a message confirming your submission is underway; for large submissions this can take a few minutes.

A progress indicator on the Submit button shows how far along your submission is while cell edits are being processed. Row and column deletions do not show intermediate progress, however if your submission only includes deletions, you will still see the loading spinner.

When your submission completes, you will see a confirmation that your changes were submitted successfully, and the grid will refresh to reflect the current state of the project. If part of your submission could not be completed, you will see a message describing what went wrong.

### Notes and limitations

* The Edit tab works only with contributions and columns that already exist in the project. There is no way to add a new row or a new column from this tab, use the Upload tab to add new data.
* Row deletions, column deletions, and cell edits can all be staged together and submitted in a single operation.

### Common errors and what they mean

<table data-search="false"><thead><tr><th>Error</th><th>What it means</th><th>What to do</th></tr></thead><tbody><tr><td>"You do not have permission to edit '<em>project</em>'."</td><td>You are not the owner of the selected project</td><td>Select a project you own, or contact the project owner</td></tr><tr><td>"Failed to load contributions: ..."</td><td>The project's data could not be loaded</td><td>Try selecting the project again; if the problem persists, contact support</td></tr><tr><td>"Error: Identifier '<em>X</em>' is already used by another row in this batch. Choose a different identifier; this edit was reverted."</td><td>You edited a cell to an identifier already used elsewhere in the grid</td><td>Choose a different identifier; your edit has already been reverted</td></tr><tr><td>"Deleting this column would remove all data from <em>n</em> contribution(s): ..."</td><td>Removing this column would leave one or more contributions with no data at all</td><td>Delete those contributions first, or make sure they retain other data, then try deleting the column again</td></tr><tr><td>"No changes detected."</td><td>You clicked Submit without making any changes</td><td>Make an edit, deletion, or column removal before submitting</td></tr></tbody></table>
