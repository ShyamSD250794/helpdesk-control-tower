# Data Dictionary

This document describes the fields present in the cleaned datasets used in the **Helpdesk Control Tower** Power BI project.

The project uses three cleaned datasets:

1. `issues_clean.csv`
2. `issues_snapshot_clean.csv`
3. `issues_change_history_clean.csv`

The dictionary preserves the field names used in the source data and explains their role within the analytical workflow.

> **Note:** Field descriptions below are based on the structure and values present in the cleaned project files. Where the exact business meaning of a field cannot be established solely from the cleaned data, the description is intentionally kept at the data-field level rather than assuming an undocumented business definition.


---

# 1. Dataset: `issues_clean.csv`

The primary cleaned issue-level dataset used for helpdesk ticket analysis.

It contains **58 fields** covering ticket identifiers, issue attributes, dates, assignee information, workflow durations, and processing information.


## 1.1 Issue Identification & Classification

| Field | Description | Analytical Use |
|---|---|---|
| `id` | Identifier associated with the issue record. | Used to identify and count issue records. |
| `issue_num` | Issue/ticket number. | Used to identify individual tickets. |
| `issue_proj` | Project associated with the issue. | Enables project-level filtering and analysis. |
| `issue_type` | Type/category of issue. | Used to segment ticket populations by issue type. |
| `issue_priority` | Priority assigned to the issue. | Used for priority analysis and filtering. |
| `issue_status` | Current status represented in the cleaned dataset. | Used for status-based analysis and KPI calculations. |
| `issue_resolution` | Resolution value associated with the issue. | Used to analyze how issues were resolved. |


## 1.2 People & Participation

| Field | Description | Analytical Use |
|---|---|---|
| `issue_reporter` | Reporter associated with the issue. | Identifies the person/account that reported the issue. |
| `issue_assignee` | Assignee associated with the issue. | Used for workload and assignee-performance analysis. |
| `issue_contr_count` | Count of contributors associated with the issue. | Provides an indication of participation associated with an issue. |


## 1.3 Ticket Dates & Lifecycle

| Field | Description | Analytical Use |
|---|---|---|
| `started` | Start timestamp/value associated with issue processing. | Used as part of ticket lifecycle and duration analysis. |
| `ended` | End timestamp/value associated with issue processing. | Used with lifecycle fields when analyzing processing duration. |
| `issue_created` | Date/time when the issue was created. | Used for ticket-volume trends and date filtering. |
| `issue_resolution_date` | Date/time associated with issue resolution. | Used for resolution-time analysis. |
| `last_change_date` | Date/time of the latest recorded change associated with the issue. | Used to understand the latest recorded activity/change. |


## 1.4 Interaction & Activity

| Field | Description | Analytical Use |
|---|---|---|
| `issue_comments_count` | Number of comments associated with the issue. | Provides an indication of issue discussion/activity. |
| `processing_steps` | Number/value representing processing steps associated with the issue. | Used as a workflow/process measure. |


---

# 2. Workflow Fields

The cleaned issue dataset contains workflow fields representing time spent in specific workflow states.

These fields use two naming patterns:

- `wf_...`
- `wfe_...`

The workflow fields are retained separately because they represent different recorded workflow measures in the dataset.

## 2.1 Workflow State Fields

| Field | Workflow State |
|---|---|
| `wf_in_review` | In Review |
| `wf_deployment` | Deployment |
| `wf_resolved` | Resolved |
| `wf_open` | Open |
| `wf_monitoring` | Monitoring |
| `wf_done` | Done |
| `wf_pending_customer_approval` | Pending Customer Approval |
| `wf_rejected` | Rejected |
| `wf_testing_monitoring` | Testing / Monitoring |
| `wf_in_progress` | In Progress |
| `wf_reopened` | Reopened |
| `wf_to_do` | To Do |
| `wf_validation` | Validation |
| `wf_resolved_under_monitoring` | Resolved Under Monitoring |
| `wf_closed` | Closed |
| `wf_waiting` | Waiting |
| `wf_cancelled` | Cancelled |
| `wf_under_review` | Under Review |
| `wf_approved` | Approved |
| `wf_pending_deployment` | Pending Deployment |


## 2.2 Workflow `wfe_` Fields

The corresponding `wfe_` fields are present for the workflow states represented in the cleaned data.

| Field | Workflow State |
|---|---|
| `wfe_in_review` | In Review |
| `wfe_deployment` | Deployment |
| `wfe_resolved` | Resolved |
| `wfe_open` | Open |
| `wfe_monitoring` | Monitoring |
| `wfe_done` | Done |
| `wfe_pending_customer_approval` | Pending Customer Approval |
| `wfe_rejected` | Rejected |
| `wfe_testing_monitoring` | Testing / Monitoring |
| `wfe_in_progress` | In Progress |
| `wfe_reopened` | Reopened |
| `wfe_to_do` | To Do |
| `wfe_validation` | Validation |
| `wfe_resolved_under_monitoring` | Resolved Under Monitoring |
| `wfe_closed` | Closed |
| `wfe_waiting` | Waiting |
| `wfe_cancelled` | Cancelled |
| `wfe_under_review` | Under Review |
| `wfe_approved` | Approved |
| `wfe_pending_deployment` | Pending Deployment |


## 2.3 Total Workflow Duration

| Field | Description | Analytical Use |
|---|---|---|
| `wf_total_time` | Total workflow time recorded for the issue. | Used as an overall workflow-duration measure. |


---

# 3. Dataset: `issues_snapshot_clean.csv`

The snapshot dataset contains **60 fields**.

It contains the issue-level information found in `issues_clean.csv` together with snapshot-specific fields used to represent issue processing cycles.

## 3.1 Snapshot-Specific Fields

| Field | Description | Analytical Use |
|---|---|---|
| `idx` | Index associated with the snapshot record. | Used to distinguish snapshot records. |
| `turn` | Turn value associated with the snapshot record. | Used to distinguish or analyze processing turns/cycles. |


## 3.2 Issue-Level Fields

The snapshot dataset also contains the issue-level fields present in `issues_clean.csv`, including:

- `id`
- `started`
- `ended`
- `issue_num`
- `issue_proj`
- `issue_reporter`
- `issue_assignee`
- `issue_contr_count`
- `issue_type`
- `issue_priority`
- `issue_created`
- `issue_resolution_date`
- `issue_resolution`
- `issue_status`
- `issue_comments_count`
- `last_change_date`


## 3.3 Workflow Fields in Snapshot Data

The snapshot dataset contains the same workflow-duration field families used in the primary issue dataset, including:

- `wf_in_review`
- `wfe_in_review`
- `wf_deployment`
- `wfe_deployment`
- `wf_resolved`
- `wfe_resolved`
- `wf_open`
- `wfe_open`
- `wf_monitoring`
- `wfe_monitoring`
- `wf_done`
- `wfe_done`
- `wf_pending_customer_approval`
- `wfe_pending_customer_approval`
- `wf_rejected`
- `wfe_rejected`
- `wf_testing_monitoring`
- `wfe_testing_monitoring`
- `wf_in_progress`
- `wfe_in_progress`
- `wf_reopened`
- `wfe_reopened`
- `wf_to_do`
- `wfe_to_do`
- `wf_validation`
- `wfe_validation`
- `wf_resolved_under_monitoring`
- `wfe_resolved_under_monitoring`
- `wf_closed`
- `wfe_closed`
- `wf_waiting`
- `wfe_waiting`
- `wf_cancelled`
- `wfe_cancelled`
- `wf_under_review`
- `wfe_under_review`
- `wf_approved`
- `wfe_approved`
- `wf_pending_deployment`
- `wfe_pending_deployment`

It also contains:

- `wf_total_time`
- `processing_steps`


---

# 4. Dataset: `issues_change_history_clean.csv`

The change-history dataset contains **6 fields**.

It records changes associated with issues and provides historical context for ticket lifecycle analysis.


| Field | Description | Analytical Use |
|---|---|---|
| `id` | Identifier associated with the change-history record. | Used to identify the change-history record. |
| `issueid` | Identifier linking the history record to an issue. | Used to associate historical changes with issues. |
| `field` | Field associated with the recorded change. | Identifies which issue attribute was changed. |
| `value` | Value associated with the recorded change. | Captures the recorded value for the changed field. |
| `created` | Date/time associated with the recorded change. | Used to establish the timing of historical changes. |
| `change_group_id` | Identifier associated with a group of related changes. | Used to group related change-history records. |


---

# 5. Field Naming Convention

The project retains the original field naming convention used by the cleaned datasets.

### `issue_` prefix

Fields beginning with `issue_` generally describe attributes of the issue/ticket itself.

Examples:

- `issue_num`
- `issue_type`
- `issue_priority`
- `issue_status`
- `issue_assignee`
- `issue_created`
- `issue_resolution_date`


### `wf_` prefix

Fields beginning with `wf_` represent workflow-related measures associated with specific workflow states.

Examples:

- `wf_open`
- `wf_in_progress`
- `wf_waiting`
- `wf_resolved`
- `wf_total_time`


### `wfe_` prefix

Fields beginning with `wfe_` form the corresponding `wfe_` workflow field family.

Examples:

- `wfe_open`
- `wfe_in_progress`
- `wfe_waiting`
- `wfe_resolved`


---

# 6. Fields Relevant to the Power BI Dashboard

Although the datasets contain many fields, the dashboard focuses primarily on fields required for operational and performance analysis.

### Ticket Volume

- `id`
- `issue_num`
- `issue_created`


### Ticket Classification

- `issue_status`
- `issue_priority`
- `issue_type`
- `issue_proj`


### Assignee Workload

- `issue_assignee`
- `issue_status`
- `issue_comments_count`
- `issue_contr_count`


### Resolution Analysis

- `issue_created`
- `issue_resolution_date`
- `issue_resolution`
- `issue_status`
- `wf_resolved`
- `wf_total_time`


### Workflow Bottlenecks

- `wf_waiting`
- `wfe_waiting`
- `wf_monitoring`
- `wfe_monitoring`
- `wf_in_progress`
- `wfe_in_progress`
- `wf_total_time`
- `processing_steps`


### Date-Based Analysis

- `issue_created`
- `issue_resolution_date`
- `last_change_date`


---

# 7. Cleaned Data Considerations

The datasets used in this project are cleaned versions of the source data.

Data preparation included validation and transformation activities required to make the data suitable for Power BI analysis.

Areas addressed during preparation included:

- Data type validation
- Date standardization
- Missing and unknown values
- Workflow field handling
- Field consistency
- Analytical readiness


## Unknown Values

The project contains explicit `unknown` values in categorical fields.

For example, `issue_priority` contains an `unknown` category.

These values are retained rather than silently treating them as a valid priority category or deleting the corresponding records.

This allows the dashboard to preserve the underlying ticket population while making data-quality considerations visible.


---

# 8. Analytical Grain

The primary issue dataset is analyzed at the **issue/ticket level**.

The snapshot dataset provides a more granular representation through snapshot records and processing turns.

The change-history dataset operates at the **change-record level**, where multiple historical records can relate to the same issue.

Because the three datasets operate at different grains, they should not be treated as interchangeable tables for simple ticket counting.


---

# 9. Relationship Between the Datasets

Conceptually, the datasets can be understood as:

```text
issues_clean
     │
     │ Issue-level information
     │
     ├───────────────┐
     │               │
     ▼               ▼
issues_snapshot   issues_change_history
     │               │
     │               │
     ▼               ▼
Processing        Historical
Snapshots         Changes


The issue dataset provides the primary ticket-level analytical context.

The snapshot dataset provides additional processing-cycle information.

The change-history dataset provides historical changes associated with issues.

10. Dashboard-Oriented Data Usage

The Helpdesk Control Tower uses these fields to transform the underlying ticket data into operational metrics and visual analysis.

The major analytical areas are:

1. Ticket volume
2. Ticket completion
3. Active workload
4. Attention-required workload
5. Waiting and monitoring workload
6. Resolution performance
7. Handling efficiency
8. Workflow bottlenecks
9. Assignee workload
10. Assignee handling duration
11. Data Dictionary Scope

This document describes the fields available in the cleaned project datasets.

It is not intended to reproduce every piece of documentation from the original dataset source.

For original dataset documentation, research context, and licensing information, refer to the dataset source documented in the main project README.

Dataset Source

Help Desk Tickets — Version 3

Mendeley Data

DOI:

10.17632/btm76zndnt.3

Dataset:

https://data.mendeley.com/datasets/btm76zndnt/3

Contributor: Mohammad Abdellatif

License: CC BY 4.0
