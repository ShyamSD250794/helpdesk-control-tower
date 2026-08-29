# KPI Definitions

This document defines the KPI cards used in the **Helpdesk Control Tower** Power BI dashboard.

The KPI cards provide a high-level view of ticket volume, completion status, operational attention, and performance across the helpdesk workflow.

The KPIs are organized into two groups:

1. Operational Overview KPIs
2. Performance KPIs


---

## 1. Operational Overview KPIs

### Total Tickets

**Definition:**  
The total number of distinct helpdesk tickets included within the current dashboard filter context.

**Business purpose:**  
Provides the overall scale of the helpdesk workload and establishes the volume against which other operational KPIs can be interpreted.

**Dashboard display:**  
**66,691 tickets** in the current unfiltered view.


### Completion %

**Definition:**  
The percentage of tickets considered completed relative to the total ticket population.

**Business purpose:**  
Provides a high-level indication of how much of the overall ticket workload has reached completion.

**Dashboard display:**  
**99.05%** in the current unfiltered view.


### Active Tickets

**Definition:**  
The number of tickets classified as active according to the dashboard's ticket-status logic.

**Business purpose:**  
Shows the volume of tickets that remain active and may still require operational action.

**Dashboard display:**  
**337 tickets** in the current unfiltered view.


### Attention Tickets

**Definition:**  
The number of tickets classified by the dashboard as requiring attention.

**Business purpose:**  
Highlights tickets that may require operational follow-up, investigation, or management attention.

**Dashboard display:**  
**573 tickets** in the current unfiltered view.


### Waiting / Monitoring Tickets

**Definition:**  
The number of tickets classified within waiting or monitoring-related states.

**Business purpose:**  
Highlights tickets that are not actively progressing and may contribute to operational delays or workflow bottlenecks.

**Dashboard display:**  
**236 tickets** in the current unfiltered view.


---

## 2. Performance KPIs

The performance section contains three KPI cards:

- Resolution Performance
- Handling Efficiency
- Waiting Bottleneck

These KPIs focus on the time required to resolve, handle, or progress tickets through the helpdesk workflow.


### Resolution Performance

## Resolution Performance

### Latest Month

**Definition:**  
Represents the average resolution duration for the latest month available within the selected dashboard context.

**Business purpose:**  
Provides a recent view of how quickly helpdesk tickets are being resolved. Lower resolution duration indicates faster ticket resolution and therefore better resolution performance.

**Dashboard display:**  
**1.6 days**

### vs previous month

**Definition:**  
Shows the percentage change in resolution duration compared with the previous month.

**Dashboard display:**  
**▼ 75.4%**

**Interpretation:**  
The downward change indicates that the resolution duration decreased compared with the previous month. Since lower resolution time represents better performance, this indicates an improvement in resolution performance.

---

## Handling Efficiency

### Latest Month

**Definition:**  
Represents the average handling duration for the latest month available within the selected dashboard context.

**Business purpose:**  
Provides a recent view of how efficiently tickets are being handled. Lower handling duration indicates that tickets are requiring less active handling time.

**Dashboard display:**  
**0.81 days**

### vs previous month

**Definition:**  
Shows the percentage change in handling duration compared with the previous month.

**Dashboard display:**  
**▼ 79.0%**

**Interpretation:**  
The downward change indicates that handling duration decreased compared with the previous month. Since lower handling time indicates greater handling efficiency, this represents an improvement.

---

## Waiting Bottleneck

### Average

**Definition:**  
Represents the average waiting time associated with tickets within the selected dashboard context.

**Business purpose:**  
Highlights the amount of time tickets spend waiting within the helpdesk workflow. High waiting time can indicate process delays, approval dependencies, handoffs, or other workflow bottlenecks.

**Dashboard display:**  
**19.90 days**

### vs previous month

**Definition:**  
Shows the percentage change in average waiting time compared with the previous month.

**Dashboard display:**  
**▼ 72.4%**

**Interpretation:**  
The downward change indicates that average waiting time decreased compared with the previous month. A reduction in waiting time indicates that workflow delays have decreased.

---

## KPI Comparison Logic

The three KPI cards display the latest available performance value together with a comparison against the previous month.

For duration-based KPIs, the percentage comparison represents the relative change in the KPI value between the latest month and the previous month.

A downward percentage indicates that the measured duration has decreased.

For the duration-based KPIs used in this dashboard:

- Lower **Resolution Performance** duration indicates faster resolution.
- Lower **Handling Efficiency** duration indicates less handling time.
- Lower **Waiting Bottleneck** duration indicates less workflow waiting time.

Therefore, a downward percentage change represents an improvement for all three KPIs.

> **Note:** The dashboard displays the month-over-month comparison as a percentage. The displayed percentage should not be interpreted as an absolute change in days.


---

## 3. Month-over-Month Comparison

The performance KPI cards include a comparison against the previous month.

The comparison allows users to identify whether the corresponding performance metric has increased or decreased relative to the previous month.

The comparison is displayed beneath the relevant KPI using the label:

**vs previous month**

The comparison should be interpreted together with the primary KPI value rather than in isolation.


---

## 4. KPI Interaction

The KPI cards respond to the dashboard's interactive filter context.

The primary dashboard filters include:

- Status
- Priority
- Created Date

When the filter context changes, the applicable KPI values and supporting visualizations are recalculated.

This allows users to investigate performance for different ticket populations and time periods.


---

## 5. KPI Interpretation

The KPI cards provide an executive-level summary of helpdesk operations.

Together, they answer four fundamental operational questions:

| Operational Question | Relevant KPI |
|---|---|
| How much work exists? | Total Tickets |
| How much of the workload has been completed? | Completion % |
| How much work remains active or requires attention? | Active Tickets, Attention Tickets, Waiting / Monitoring Tickets |
| How efficiently is the helpdesk processing work? | Resolution Performance, Handling Efficiency, Waiting Bottleneck |


---

## 6. KPI Categories

### Volume

**Total Tickets**

Measures the overall ticket population.


### Completion

**Completion %**

Measures the proportion of the ticket population considered completed.


### Operational Attention

**Active Tickets**  
**Attention Tickets**  
**Waiting / Monitoring Tickets**

These KPIs highlight tickets that may require continued operational attention.


### Performance

**Resolution Performance**  
**Handling Efficiency**  
**Waiting Bottleneck**

These KPIs focus on time-based operational performance and help identify potential delays or inefficiencies.


---

## 7. Dashboard KPI Summary

| KPI | Supporting Label | Unit | Primary Purpose |
|---|---|---|---|
| Total Tickets | — | Tickets | Measure overall workload |
| Completion % | — | Percentage | Measure completed workload |
| Active Tickets | — | Tickets | Identify active workload |
| Attention Tickets | — | Tickets | Identify tickets requiring attention |
| Waiting / Monitoring Tickets | — | Tickets | Identify waiting or monitoring workload |
| Resolution Performance | Latest Month | Days | Monitor recent resolution performance |
| Handling Efficiency | Latest Month | Days | Monitor recent handling efficiency |
| Waiting Bottleneck | Average | Days | Monitor waiting-time bottlenecks |


---

## 8. Business Interpretation

The KPI cards are intended to be used as an entry point into the dashboard rather than as standalone metrics.

The high-level KPIs provide a snapshot of the operational environment, while the detailed dashboard pages allow users to investigate the underlying drivers.

For example:

- **Total Tickets** establishes the overall workload.
- **Completion %** provides a high-level view of completed work.
- **Active Tickets** indicates remaining active workload.
- **Attention Tickets** highlights tickets requiring follow-up.
- **Waiting / Monitoring Tickets** indicates tickets spending time in non-progressing or monitoring states.
- **Resolution Performance** provides a recent view of resolution duration.
- **Handling Efficiency** provides a recent view of handling duration.
- **Waiting Bottleneck** highlights average waiting time.

Together, these metrics provide a concise operational overview before users move into the detailed ticket-resolution, workflow-bottleneck, and assignee-workload analyses.


---

## 9. Important Note on KPI Definitions

The numerical values shown in this document reflect the dashboard's current unfiltered view at the time the documentation was prepared.

KPI values can change when report filters, slicers, or other interactive selections are applied.

The exact calculation logic is defined by the underlying Power BI measures and data model used in the project.
