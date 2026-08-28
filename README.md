# Helpdesk Control Tower

A Power BI dashboard designed to analyze helpdesk ticket operations, resolution performance, workflow bottlenecks, and assignee workload.

---

## 📊 Dashboard Demo

![Dashboard Walkthrough](PowerBI/Screenshots/Demo/Dashboard_Walkthrough.gif)

---

## 📌 Project Overview

The **Helpdesk Control Tower** transforms raw helpdesk ticket data into an interactive operational analytics dashboard.

The project focuses on four key areas:

- Ticket volume and operational trends
- Resolution and handling performance
- Workflow bottlenecks and waiting time
- Assignee workload and productivity

The dashboard is designed from an operations-management perspective, helping identify workload concentration, resolution delays, active tickets, attention-required tickets, and process bottlenecks.

---

## 🖥️ Dashboard Pages

### 1. Overview

Provides a high-level operational view of the helpdesk environment.

#### Key KPIs

- Total Tickets
- Completion %
- Active Tickets
- Attention Tickets
- Waiting / Monitoring Tickets
- Latest Month Resolution
- Latest Month Handling
- Average Waiting Time

#### Key Visuals

- Monthly Ticket Volume & Trend
- Resolution Time by Ticket Cohort
- Resolution Performance
- Handling Efficiency
- Waiting Bottleneck

---

### 2. Ticket Resolution

Focuses on ticket resolution performance and resolution-time patterns.

The page analyzes:

- Resolution duration
- Resolution trends
- Ticket cohorts
- Resolution performance
- Ticket volume by resolution period

---

### 3. Workflow Bottlenecks

Analyzes the different stages of the ticket workflow to identify delays, waiting periods, and operational bottlenecks.

The analysis focuses on:

- Workflow stage duration
- Waiting time
- Processing time
- Bottleneck stages
- Tickets requiring monitoring or additional attention

---

### 4. Assignee & Workload

Analyzes workload distribution and handling performance across helpdesk assignees.

#### Key Visuals

- Top 5 Assignees by Ticket Volume
- Median Handling Duration by Top Assignees
- Workload vs Handling Duration
- Ticket Status Mix by Top Assignees
- Assignee Performance Table

---

## 🛠️ Tools & Technologies

- Microsoft Power BI
- Power Query
- DAX
- Microsoft Excel
- Data Cleaning & Transformation
- Data Profiling
- Data Visualization
- Operational Analytics

---

## 🔄 Data Preparation

The original helpdesk dataset required preprocessing before it could be used effectively for dashboard analysis.

The data preparation workflow included:

1. Data profiling
2. Data type validation
3. Date standardization
4. Handling missing and unknown values
5. Workflow field transformation
6. Data cleaning
7. Creation of analytical measures
8. KPI validation
9. Loading transformed data into Power BI

Power Query was used as the primary data-transformation layer before the cleaned data was loaded into the Power BI model.

---

## 📈 Analytical Approach

The dashboard combines descriptive and diagnostic analytics.

It answers questions such as:

- How many tickets are being handled?
- How is ticket volume changing over time?
- What percentage of tickets have been completed?
- How many tickets remain active?
- Which tickets require attention?
- How many tickets are waiting or under monitoring?
- Which assignees handle the largest workloads?
- Which assignees have longer handling durations?
- Where are tickets spending the most time?
- Which workflow stages create bottlenecks?
- Is high workload associated with longer handling duration?
- How does resolution performance change over time?

---

## 🎯 Business Value

The dashboard can help helpdesk and operations teams:

- Monitor operational workload
- Identify overloaded assignees
- Detect resolution delays
- Identify workflow bottlenecks
- Prioritize attention-required tickets
- Monitor waiting and monitoring queues
- Compare assignee workloads
- Evaluate handling efficiency
- Support workload-balancing decisions
- Improve operational visibility

---

# 📂 Dataset

## Help Desk Tickets Dataset

This project uses the **Help Desk Tickets** dataset published on **Mendeley Data**.

**Dataset:** Help Desk Tickets  
**Version:** 3  
**Published:** 3 August 2026  
**DOI:** 10.17632/btm76zndnt.3  
**Contributor:** Mohammad Abdellatif  

### Dataset Source

https://data.mendeley.com/datasets/btm76zndnt/3

The dataset was created as part of a study involving an experiment with a helpdesk team at an international software company.

The original study aimed to implement an automated performance appraisal model that evaluates helpdesk team performance using issue reports and features derived from classifying messages exchanged with customers using Dialog Acts.

The data was extracted from a PostgreSQL database and curated to provide aggregated views of helpdesk tickets reported between **January 2007 and March 2023**.

Certain fields have been anonymized or masked to protect the privacy of the original data owner while preserving the overall meaning of the information.

---

## 📦 Dataset Contents

The Mendeley dataset contains several related files:

### `issues.csv`

Contains information about reported tickets, including:

- Ticket category
- Priority
- Reporter
- Related project
- Assigned helpdesk employee
- Start time
- Resolution time
- Time spent in different resolution steps

### `issues_change_history.csv`

Contains ticket assignee and status change history.

This historical information can be used to calculate the amount of time spent at different stages of the ticket lifecycle.

### `issues_snapshots.csv`

Contains the same underlying issue records while accounting for tickets handled by multiple assignees.

Each record represents a processing cycle for an assignee.

### `scored_issues_snapshot_sample.xlsx`

Contains a stratified and representative sample of tickets that was provided to a helpdesk manager for performance appraisal.

The resolution performance was evaluated against three targets using scores from **1 to 5**, where 5 represents the highest score.

### `sample_utterances.csv`

Contains curated messages exchanged between ticket reporters and the helpdesk team.

The messages correspond to issues included in the scored issue sample and were used as part of the original Dialog Act classification study.

### `holidays_by_country.csv`

Contains holiday information between **2007 and 2023** for countries associated with reported tickets.

---

## 📚 Dataset Documentation

The original dataset also provides documentation and supporting files explaining how the datasets should be interpreted.

These include:

- `FEATURES.md` — descriptions of dataset features and fields
- `EXAMPLE.md` — example of an issue across the different datasets
- `process-flow.png` — demonstration of the helpdesk issue-resolution process
- Database documentation describing the extraction process

---

## 🔬 Original Dataset Research Context

The dataset was created for research into automated performance appraisal for helpdesk teams.

The original research explored areas including:

- Performance appraisal
- Data mining
- Machine learning
- Dialog Act classification
- Classification
- Clustering
- Regression
- Natural language processing
- Association rule mining

The associated research repository describes the original work as:

**Toward Performance Appraisal Automation – A Harmonized Machine Learning Model Utilizing Dialog Act Classification**

The original implementation used data extracted from a PostgreSQL-based helpdesk system and performed data engineering, preprocessing, feature extraction, sampling, clustering, and machine-learning experiments.

---

## 🧹 Dataset Usage in This Project

This project does **not** attempt to reproduce the original machine-learning research.

Instead, the dataset is repurposed for a practical **Business Intelligence and Helpdesk Operations Analytics** use case.

The project focuses on transforming the ticket-level operational data into a Power BI dashboard that allows users to investigate:

- Ticket volume
- Ticket lifecycle
- Resolution performance
- Handling duration
- Waiting time
- Workflow bottlenecks
- Assignee workload
- Operational status

This demonstrates how a research-oriented dataset can be transformed into a practical business reporting and decision-support solution.

---

## 📜 Dataset Attribution

The underlying dataset is attributed to:

**Mohammad Abdellatif**

Dataset:

**Help Desk Tickets, Version 3**

Mendeley Data

DOI:

`10.17632/btm76zndnt.3`

License:

**CC BY 4.0**

Dataset source:

https://data.mendeley.com/datasets/btm76zndnt/3

The dataset should be credited to its original contributor when reused or redistributed.

---

# 📁 Repository Structure

```text
helpdesk-control-tower/
│
├── README.md
│
├── PowerBI/
│   │
│   ├── Helpdesk_Control_Tower.pbix
│   │
│   └── Screenshots/
│       │
│       ├── Overview.png
│       ├── Ticket_Resolution.png
│       ├── Workflow_Bottlenecks.png
│       ├── Assignee_Workload.png
│       │
│       └── Demo/
│           └── Dashboard_Walkthrough.gif
│
└── Documentation/
    │
    ├── Data_Dictionary.md
    ├── Data_Cleaning.md
    ├── DAX_Measures.md
    └── Interview_QA.md



👤 Author
Shyam Das

Business Intelligence / Data Analytics Portfolio Project

Project Focus
Power BI Dashboard Development
Power Query Data Transformation
DAX
Data Cleaning
Data Profiling
Operational Analytics
KPI Development
Business Intelligence Reporting

GitHub:

https://github.com/ShyamSD250794


📌 Disclaimer

This project is an independent portfolio project created using a publicly available and anonymized helpdesk dataset.

The dashboard, data transformations, analytical model, KPIs, visualizations, and business interpretation presented in this repository were developed independently for portfolio and learning purposes.

The project is not affiliated with the organization from which the original helpdesk data was collected.

The underlying dataset remains subject to its original licensing and attribution requirements.
