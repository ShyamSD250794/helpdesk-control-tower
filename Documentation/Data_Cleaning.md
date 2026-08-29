# Data Cleaning & Transformation

## 1. Overview

The Helpdesk Control Tower project uses Power Query in Power BI to transform
the source helpdesk ticket data into analysis-ready views for operational
reporting.

The overall data pipeline for the project is:

Raw Helpdesk Data
        ↓
Data Profiling
        ↓
Power Query Transformation
        ↓
Analytical Views
        ↓
DAX Measures
        ↓
Power BI Dashboard

The objective of the cleaning and transformation process was not to
arbitrarily modify the source data. Instead, the process focused on
understanding the source structure, identifying data-quality issues,
standardizing fields required for analysis, creating appropriate working
columns, and producing focused analytical views for the dashboard.

---

# 2. Source Dataset

## Dataset

**Help Desk Tickets**

**Author:** Mohammad Abdellatif

**Mendeley Data:**  
https://data.mendeley.com/datasets/btm76zndnt/3

**Version used for reference:** Version 3

The dataset was extracted from a PostgreSQL-based helpdesk system and
contains information relating to helpdesk tickets, including ticket
categories, priorities, reporters, projects, assignees, start and
resolution times, and time spent in different resolution/workflow steps.

The dataset also includes supporting data such as assignee/status change
history and ticket snapshots. The snapshots may contain multiple records
for tickets handled by multiple assignees because each record represents a
processing cycle.

The source dataset contains anonymized/masked values so that the original
data owner's information is protected while preserving the analytical
meaning of the fields.

---

# 3. Initial Source Structure

The primary ticket data used during the project was brought into the
working environment as `tblIssues`.

The source contained fields covering several categories of information:

### Ticket identification

- `id`
- `issue_num`
- `issue_proj`

### Ticket participants

- `issue_reporter`
- `issue_assignee`

### Ticket classification

- `issue_type`
- `issue_priority`
- `issue_status`

### Ticket timing

- `issue_created`
- `issue_resolution_date`
- `started`
- `ended`
- `last_change_date`

### Ticket activity

- `issue_comments_count`
- `issue_contr_count`

### Workflow information

The source also contained multiple workflow-related fields, including
workflow states and their corresponding timing information.

Examples include:

- `wf_open`
- `wf_pending_customer_approval`
- `wf_in_progress`
- `wf_in_review`
- `wf_pending_deployment`
- `wf_deployment`
- `wf_testing_monitoring`
- `wf_monitoring`
- `wf_resolved`
- `wf_done`
- `wf_closed`
- `wf_reopened`
- `wf_rejected`
- `wf_validation`
- `wf_waiting`
- `wf_under_review`
- `wf_total_time`

These fields were important for analysing workflow bottlenecks and
resolution performance.

---

# 4. Data Profiling

Before transforming the data, the source was profiled to understand its
structure and identify fields requiring attention.

A `Data_Profiling` worksheet was used during the earlier profiling stage.

The profiling process examined:

- number of records;
- column names;
- data types;
- unique values;
- missing/null values;
- categorical distributions;
- date ranges;
- potential duplicate values;
- fields requiring transformation;
- fields relevant to the final dashboard.

This profiling step was important because the source was not treated as
a perfectly clean analytical table.

---

# 5. Source Volume and Cardinality

The profiling work identified approximately **66,691 records** for the
`id` field.

The profiling also showed that different fields had substantially
different cardinalities.

Examples from the profiling work included:

| Field | Observation |
|---|---|
| `id` | 66,691 unique values |
| `issue_assignee` | 381 unique values |
| `issue_reporter` | 1,114 unique values |
| `issue_priority` | Multiple categorical values plus `unknown` |
| `ended` | 65,786 unique values |

The exact number of unique values is not treated as a data-quality
problem by itself. Cardinality was recorded to understand the structure
of the data and to identify fields that could behave differently in
analysis.

---

# 6. Categorical Data Profiling

Categorical fields were examined for their distributions before being
used in dashboard analysis.

For example, the profiling of `issue_priority` identified:

| Priority | Count |
|---|---:|
| Blocker | 656 |
| High | 4,554 |
| Highest | 2,084 |
| Low | 560 |
| Lowest | 84 |
| Medium | 24,788 |
| unknown | 33,965 |

The `unknown` category was not automatically converted into another
priority because doing so would introduce an unsupported assumption about
the source data.

It was therefore treated as an existing source category rather than
inventing a priority classification.

---

# 7. Date and Datetime Issues

Date fields required particular attention during the profiling and
transformation process.

The source contained date/datetime values in formats that were not
immediately suitable for consistent Power BI date analysis.

The profiling work identified formatting variations involving:

- periods;
- slashes;
- hyphens;
- datetime strings containing time information.

The `issue_created` field was therefore examined separately to determine
the usable date component.

The profiling also identified the date range represented by the source
values.

The minimum date extracted from the relevant `issue_created` values was
recorded as Excel serial value `39187`, while the maximum was `44999`.

These values correspond to the date range represented in the source after
extracting the date component.

---

# 8. Date Transformation Strategy

The project did not simply overwrite the original datetime fields.

Where a date-only value was required, the relevant field was duplicated
and transformed.

This allowed the original information to remain available while creating
a field suitable for date-based analysis or display.

Power Query was used to:

- extract date components;
- convert fields to appropriate date/datetime types;
- create display-oriented date fields;
- retain analytical date fields separately from presentation fields.

---

# 9. Important Distinction: Duplicated Columns vs Duplicate Rows

Several Power Query queries contain steps named:

- `Duplicated Column`
- `Duplicated Column1`

These steps **do not mean that duplicate records were removed**.

They indicate that an existing column was copied so that the copy could
be transformed while preserving the original field.

This distinction is important because the project documentation should
not claim that duplicate tickets were removed simply because a Power
Query step contains the word "Duplicated".

---

# 10. Duplicate-Record Handling

No `Remove Duplicates` step is currently documented in the Power Query
workflow used for the dashboard.

Therefore, the project does **not** claim that duplicate source rows were
globally removed.

This is especially important for this dataset because the source itself
contains structures in which ticket records may legitimately occur more
than once.

The Mendeley documentation explains that `issues_snapshots.csv` contains
records for tickets handled by multiple assignees, with each record
representing a processing cycle per assignee.

Consequently, blindly removing duplicate-looking ticket records could
destroy legitimate workflow information.

The project therefore avoids claiming duplicate removal where it was not
actually performed.

---

# 11. Missing and Null Values

The profiling stage identified fields containing missing/null values.

Null values were not globally replaced simply to make the dataset appear
cleaner.

A missing assignee, for example, can represent a meaningful state in the
source data and should not automatically be converted into a fabricated
person or category.

Therefore:

- null values were profiled;
- relevant fields were assessed in context;
- no blanket null-to-zero or null-to-text replacement is claimed;
- missing values were not automatically treated as errors.

This preserves the meaning of the source data.

---

# 12. Power Query Analytical Architecture

After the initial source/profiling stage, the Power BI model uses
dedicated Power Query views for different analytical requirements.

The current Power Query workspace contains:

- `vw_Ticket_Summary`
- `vw_Ticket_Resolution`
- `vw_Workflow_Analysis`
- `vw_Assignee_Workload`
- `Refresh Metadata`

The `vw_` naming convention identifies these as analytical views.

This structure prevents every dashboard visual from having to work directly
with the entire raw source structure.

---

# 13. `vw_Ticket_Summary`

## Purpose

`vw_Ticket_Summary` provides a ticket-level analytical view used for
summary and dashboard reporting.

## Applied steps

The query currently contains the following Power Query steps:

1. `Source`
2. `Navigation`
3. `Duplicated Column`
4. `Renamed Columns`
5. `Changed Type`
6. `Duplicated Column1`
7. `Renamed Columns1`
8. `Changed Type1`
9. `Removed Columns`
10. `Changed Type2`
11. `Removed Columns1`
12. `Added Custom`

These steps represent the actual transformation sequence visible in the
Power Query query.

---

# 14. Ticket Summary Column Transformations

Column duplication was used to create working copies of source fields
before applying transformations.

Columns were then renamed where necessary to make their purpose clearer.

Data types were explicitly changed so that Power BI would interpret the
resulting fields correctly.

Unnecessary fields were subsequently removed from the analytical view.

A custom field was also added for resolution-date display.

The custom transformation used the following logic:

    Date.ToText(
        Date.From([issue_resolution_date]),
        "dd-MMM-yyyy"
    )

This converts the resolution datetime into a formatted display value such
as:

`06-Jan-2016`

The display field is separate from the underlying date value used for
analysis.

---

# 15. Ticket Resolution Transformation

A dedicated Power Query view was created for ticket-resolution analysis:

`vw_Ticket_Resolution`

The view was designed to contain the fields required for analysing resolved
tickets and their resolution timing.

The query contains the following transformation steps:

1. `Source`
2. `Navigation`
3. `Duplicated Column`
4. `Renamed Columns`
5. `Extracted Date`
6. `Changed Type`
7. `Renamed Columns1`
8. `Removed Columns`

The **Extracted Date** step separates the date component from the relevant
datetime field so that the resulting value can be used for date-based
analysis.

The **Changed Type** step ensures that the extracted value is treated as an
appropriate date/data type in Power BI.

The final **Removed Columns** step removes fields that are no longer required
in the analytical view.

The resulting `vw_Ticket_Resolution` query provides a focused dataset for
analysing ticket resolution performance rather than carrying the complete
source structure into the dashboard.

---

# 16. Creating a Resolution Date for Analysis

The resolution datetime was transformed into a proper date value where
required for analytical use.

The purpose of this transformation is to separate:

- The underlying date used for calculations and filtering
- The formatted display value used for presentation

This distinction is important because formatted text should not be used as a
substitute for an actual date field in analytical calculations.

The analytical date field can be used for:

- Monthly grouping
- Latest-month identification
- Previous-month comparison
- Date filtering
- Time-based analysis

The formatted resolution-date field is used for readable presentation, while
the underlying date value remains available for analytical calculations.

---

# 17. Building the Ticket Resolution View

A dedicated Power Query view was created for ticket-resolution analysis:

`vw_Ticket_Resolution`

The view was designed to contain the fields required for analysing resolved
tickets and their resolution timing.

The query contains transformations including:

- Source navigation
- Column duplication where required
- Column renaming
- Date extraction
- Data type changes
- Removal of temporary or presentation-only columns

The resulting view contains a focused set of fields for resolution analysis
rather than carrying the full source structure into the dashboard.

---

# 18. Building the Ticket Summary View

A separate analytical view was created:

`vw_Ticket_Summary`

This view was designed as a compact ticket-level dataset for dashboard
analysis.

The query contains transformations including:

- Source navigation
- Column duplication
- Column renaming
- Data type changes
- Removal of intermediate columns
- Creation of the resolution display field

The final query was kept narrower than the original source dataset so that
the dashboard could work with fields relevant to ticket-level analysis.

---

# 19. Removing Intermediate / Temporary Columns

Some columns were created temporarily during transformation.

After the required transformation had been completed, these intermediate
columns were removed from the final analytical views.

This was done to avoid carrying unnecessary helper columns into the Power BI
model.

The removal of a temporary column does not mean that the original source data
was deleted.

It means that the intermediate field was no longer required in the final
query output.

---

# 20. Data Type Standardisation

Data types were explicitly changed during the Power Query process where
required.

This was particularly important for:

- Numeric identifiers
- Date fields
- Datetime fields
- Calculated or derived fields

Correct data types ensure that Power BI can perform calculations, sorting,
filtering and time-based analysis correctly.

For example, a resolution date should remain a date/datetime value for
analytical purposes rather than being converted to text merely for display.

---

# 21. Duplicate Handling

No blanket duplicate-removal operation was applied to the source dataset.

This is intentional.

The presence of repeated values in individual columns does not automatically
mean that a ticket record is a duplicate.

Fields such as:

- `issue_reporter`
- `issue_assignee`
- `issue_proj`
- `issue_type`
- `issue_priority`
- `issue_status`

are expected to contain repeated values across many legitimate tickets.

Therefore, repeated values were not removed simply because they appeared more
than once.

No claim is made that duplicate source records were removed unless an explicit
deduplication transformation was actually applied.

---

# 22. Null and Missing Values

Missing values were retained where they represented legitimate missing
information in the source data.

For example, some records contain blank or null values for fields such as
`issue_assignee`.

A missing assignee does not necessarily represent a bad record. It may indicate
that the ticket had not been assigned or that the source system did not contain
an assignee value.

Therefore, null values were not indiscriminately replaced with artificial
values.

This prevents the cleaning process from introducing information that was not
present in the source data.

---

# 23. Source Columns vs Analytical Columns

The original dataset contains substantially more fields than are required for
every dashboard view.

Rather than forcing every source column into every analytical view, the
Power Query layer was used to create focused datasets for specific analytical
purposes.

The project therefore uses separate views for different dashboard areas,
including:

- `vw_Ticket_Summary`
- `vw_Ticket_Resolution`
- `vw_Workflow_Analysis`
- `vw_Assignee_Workload`

This approach keeps the Power BI model more focused and makes each analytical
view easier to understand.

---

# 24. Refresh Metadata

A separate query named:

`Refresh Metadata`

was retained as part of the Power Query structure.

This provides supporting metadata for the refresh process rather than forming
part of the primary ticket-level analytical dataset.

---

# 25. Final Power Query Output

The cleaned and transformed data is not represented by a single copy of the
original raw table.

Instead, Power Query acts as the transformation layer between the source data
and the Power BI analytical model.

The workflow can therefore be represented as:

**Source Dataset**

↓

**Power Query Transformations**

↓

**Analytical Views**

↓

**Power BI Data Model**

↓

**Dashboard KPIs and Visualisations**

---

# 26. Cleaning Philosophy

The objective of the cleaning process was not to make the dataset artificially
perfect.

The objective was to make the source data:

- Structurally consistent
- Correctly typed
- Suitable for analysis
- Easier to interpret
- Appropriate for Power BI modelling
- Suitable for dashboard presentation

Where the source contained legitimate missing information, that information
was not automatically fabricated or overwritten.

Where a transformation was required for analysis or presentation, it was
performed in Power Query rather than manually altering the source dataset.

---

# 27. Important Note on the Original Dataset

The project uses the Helpdesk Tickets dataset published on Mendeley Data.

The original dataset contains helpdesk ticket information including ticket
identifiers, projects, reporters, assignees, priorities, statuses, issue
types, dates and workflow-related information.

The Power BI project does not claim that the original dataset was completely
free of data-quality issues.

Instead, this document records the transformations that were actually
performed before the data was used for reporting.

---

# 28. Final Cleaning Result

The final Power Query layer provides structured analytical views that support
the dashboard's primary areas:

- Ticket resolution analysis
- Workflow analysis
- Assignee workload analysis
- Ticket-level summary analysis

These outputs are then used by the Power BI model to calculate and display
the project's key performance indicators:

- **Resolution Performance**
- **Handling Efficiency**
- **Waiting Bottleneck**

The cleaning process therefore serves as the preparation layer between the
raw helpdesk dataset and the final Helpdesk Control Tower dashboard.

---

# Summary

The overall data-preparation workflow is:

**Mendeley Helpdesk Dataset**

→ **`tblIssues`**

→ **Data Profiling**

→ **Power Query Cleaning & Transformation**

→ **Focused Analytical Views**

→ **Power BI Data Model**

→ **KPIs & Dashboard**

The documented transformations focus on data typing, date handling, column
management, analytical view creation and presentation-ready fields while
preserving legitimate source information such as nulls and repeated category
values.
