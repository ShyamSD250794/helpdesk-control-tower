# DAX Measures

## Helpdesk Control Tower

The dashboard uses DAX measures to calculate ticket volumes, operational performance KPIs, month-over-month comparisons and workflow metrics.

Power Query handles data preparation and transformation, while DAX provides the analytical calculation layer.

---

## 1. Core Ticket Measures

### Total Tickets

Counts the total number of tickets in the current filter context.

### Active Tickets

Calculates tickets classified as active.

### Completed Tickets

Calculates tickets classified as completed.

### Attention Tickets

Combines active tickets with waiting/monitoring tickets.

### Completion %

Calculates the proportion of tickets that have been completed.

---

## 2. Resolution Performance

### Average Resolution Duration (Days)

Calculates the average resolution duration while excluding blank values.

```DAX
Average Resolution Duration (Days) =
AVERAGEX(
    FILTER(
        vw_Ticket_Resolution,
        NOT ISBLANK(vw_Ticket_Resolution[resolution_days])
    ),
    vw_Ticket_Resolution[resolution_days]
)
```

### Latest Month Resolution (Days)

Calculates the average resolution duration for the latest available month.

```DAX
Latest Month Resolution (Days) =
VAR LatestDate =
    MAXX(
        ALL(Calendar),
        Calendar[Date]
    )
RETURN
    CALCULATE(
        [Average Resolution Duration (Days)],
        DATESBETWEEN(
            Calendar[Date],
            EOMONTH(LatestDate, -1) + 1,
            LatestDate
        )
    )
```

### Previous Month Resolution

Calculates the corresponding resolution duration for the previous month.

```DAX
Previous Month Resolution =
VAR LatestDate =
    MAXX(
        ALL(Calendar),
        Calendar[Date]
    )
VAR PreviousMonthEnd =
    EOMONTH(LatestDate, -1)
RETURN
    CALCULATE(
        [Average Resolution Duration (Days)],
        DATESBETWEEN(
            Calendar[Date],
            EOMONTH(PreviousMonthEnd, -1) + 1,
            PreviousMonthEnd
        )
    )
```

### Resolution Change %

Calculates the percentage change between the latest and previous month.

```DAX
Resolution Change % =
DIVIDE(
    [Latest Month Resolution (Days)] - [Previous Month Resolution],
    [Previous Month Resolution]
)
```

---

## 3. Handling Efficiency

### Average Handling Duration (Days)

Calculates the average handling duration while excluding blank values.

```DAX
Average Handling Duration (Days) =
AVERAGEX(
    FILTER(
        vw_Assignee_Workload,
        NOT ISBLANK(vw_Assignee_Workload[handling_days])
    ),
    vw_Assignee_Workload[handling_days]
)
```

### Latest Month Handling (Days)

Calculates the average handling duration for the latest available month.

```DAX
Latest Month Handling (Days) =
VAR LatestDate =
    MAXX(
        ALL(Calendar),
        Calendar[Date]
    )
RETURN
    CALCULATE(
        [Average Handling Duration (Days)],
        DATESBETWEEN(
            Calendar[Date],
            EOMONTH(LatestDate, -1) + 1,
            LatestDate
        )
    )
```

### Previous Month Handling (Days)

Calculates the average handling duration for the previous month.

```DAX
Previous Month Handling (Days) =
VAR LatestDate =
    MAXX(
        ALL(Calendar),
        Calendar[Date]
    )
VAR PreviousMonthEnd =
    EOMONTH(LatestDate, -1)
RETURN
    CALCULATE(
        [Average Handling Duration (Days)],
        DATESBETWEEN(
            Calendar[Date],
            EOMONTH(PreviousMonthEnd, -1) + 1,
            PreviousMonthEnd
        )
    )
```

### Handling Change %

Calculates the month-over-month percentage change in handling duration.

```DAX
Handling Change % =
DIVIDE(
    [Latest Month Handling (Days)] -
    [Previous Month Handling (Days)],
    [Previous Month Handling (Days)]
)
```

---

## 4. Waiting Bottleneck

### Average Waiting Time (Days)

Calculates the average waiting time while excluding blank values.

```DAX
Average Waiting Time (Days) =
AVERAGEX(
    FILTER(
        vw_Workflow_Analysis,
        NOT ISBLANK(vw_Workflow_Analysis[waiting_days])
    ),
    vw_Workflow_Analysis[waiting_days]
)
```

### Latest Month Waiting (Days)

Calculates the average waiting time for the latest available month.

```DAX
Latest Month Waiting (Days) =
VAR LatestDate =
    MAXX(
        ALL(Calendar),
        Calendar[Date]
    )
RETURN
    CALCULATE(
        [Average Waiting Time (Days)],
        DATESBETWEEN(
            Calendar[Date],
            EOMONTH(LatestDate, -1) + 1,
            LatestDate
        )
    )
```

### Previous Month Waiting (Days)

Calculates the average waiting time for the previous month.

```DAX
Previous Month Waiting (Days) =
VAR LatestDate =
    MAXX(
        ALL(Calendar),
        Calendar[Date]
    )
VAR PreviousMonthEnd =
    EOMONTH(LatestDate, -1)
RETURN
    CALCULATE(
        [Average Waiting Time (Days)],
        DATESBETWEEN(
            Calendar[Date],
            EOMONTH(PreviousMonthEnd, -1) + 1,
            PreviousMonthEnd
        )
    )
```

### Waiting Change %

Calculates the month-over-month percentage change in waiting time.

```DAX
Waiting Change % =
DIVIDE(
    [Latest Month Waiting (Days)] -
    [Previous Month Waiting (Days)],
    [Previous Month Waiting (Days)]
)
```

---

## 5. Ticket Volume Comparison

### Previous Month Tickets

Returns the ticket count for the previous month.

```DAX
Previous Month Tickets =
CALCULATE(
    [Total Tickets],
    DATEADD(Calendar[Date], -1, MONTH)
)
```

### Ticket Volume Change %

Calculates the month-over-month change in ticket volume.

```DAX
Ticket Volume Change % =
DIVIDE(
    [Total Tickets] - [Previous Month Tickets],
    [Previous Month Tickets]
)
```

### 3 Month Avg Tickets

Calculates the average monthly ticket volume across the latest three-month period.

```DAX
3 Month Avg Tickets =
VAR CurrentDate =
    MAX(Calendar[Date])
RETURN
    AVERAGEX(
        DATESINPERIOD(
            Calendar[Date],
            CurrentDate,
            -3,
            MONTH
        ),
        CALCULATE([Total Tickets])
    )
```

---

## 6. Workflow Analysis

### Tickets in Workflow State

Returns the number of tickets that spent time in the selected workflow state.

```DAX
Tickets in Workflow State =
VAR SelectedState =
    SELECTEDVALUE('Workflow States'[Workflow State])
RETURN
    SWITCH(
        SelectedState,
        "Open",
            COUNTROWS(
                FILTER(
                    vw_Workflow_Analysis,
                    vw_Workflow_Analysis[open_days] > 0
                )
            ),
        "In Progress",
            COUNTROWS(
                FILTER(
                    vw_Workflow_Analysis,
                    vw_Workflow_Analysis[in_progress_days] > 0
                )
            ),
        "Pending Deployment",
            COUNTROWS(
                FILTER(
                    vw_Workflow_Analysis,
                    vw_Workflow_Analysis[pending_deployment_days] > 0
                )
            ),
        "Validation",
            COUNTROWS(
                FILTER(
                    vw_Workflow_Analysis,
                    vw_Workflow_Analysis[validation_days] > 0
                )
            ),
        "Waiting",
            COUNTROWS(
                FILTER(
                    vw_Workflow_Analysis,
                    vw_Workflow_Analysis[waiting_days] > 0
                )
            ),
        BLANK()
    )
```

### Total Workflow State Days

Sums the time spent in the selected workflow state.

```DAX
Total Workflow State Days =
VAR SelectedState =
    SELECTEDVALUE('Workflow States'[Workflow State])
RETURN
    SWITCH(
        SelectedState,
        "Open", SUM(vw_Workflow_Analysis[open_days]),
        "In Progress",
            SUM(vw_Workflow_Analysis[in_progress_days]),
        "Pending Deployment",
            SUM(vw_Workflow_Analysis[pending_deployment_days]),
        "Validation",
            SUM(vw_Workflow_Analysis[validation_days]),
        "Waiting",
            SUM(vw_Workflow_Analysis[waiting_days]),
        BLANK()
    )
```

### Average Workflow State Days

Calculates average time spent in the selected workflow state.

```DAX
Average Workflow State Days =
VAR SelectedState =
    SELECTEDVALUE('Workflow States'[Workflow State])
RETURN
    SWITCH(
        SelectedState,
        "Open", AVERAGE(vw_Workflow_Analysis[open_days]),
        "In Progress",
            AVERAGE(vw_Workflow_Analysis[in_progress_days]),
        "Pending Deployment",
            AVERAGE(vw_Workflow_Analysis[pending_deployment_days]),
        "Validation",
            AVERAGE(vw_Workflow_Analysis[validation_days]),
        "Waiting",
            AVERAGE(vw_Workflow_Analysis[waiting_days]),
        BLANK()
    )
```

### Average Workflow Time Days

Calculates average total workflow time.

```DAX
Average Workflow Time Days =
AVERAGE(vw_Workflow_Analysis[total_workflow_days])
```

---

## 7. Assignee Workload

### Tickets by Assignee

Counts distinct tickets associated with each assignee.

```DAX
Tickets by Assignee =
DISTINCTCOUNT(
    'vw_Assignee_Workload'[ticket_id]
)
```

### Active Tickets by Assignee

Counts distinct tickets that are not closed, done or resolved.

```DAX
Active Tickets by Assignee =
CALCULATE(
    DISTINCTCOUNT('vw_Assignee_Workload'[ticket_id]),
    'vw_Assignee_Workload'[issue_status] <> "closed",
    'vw_Assignee_Workload'[issue_status] <> "done",
    'vw_Assignee_Workload'[issue_status] <> "resolved"
)
```

---

## 8. Additional Statistical Measures

The model also contains supporting measures for median and maximum values.

Key measures include:

- `Median Resolution Duration (Days)`
- `Median Handling Days`
- `Median Workflow State Days`
- `Median Workflow Time Days`
- `Max Resolution Days`
- `Completion Remaining %`

Examples:

```DAX
Median Resolution Duration (Days) =
MEDIANX(
    FILTER(
        vw_Ticket_Resolution,
        NOT ISBLANK(vw_Ticket_Resolution[resolution_days])
    ),
    vw_Ticket_Resolution[resolution_days]
)
```

```DAX
Median Handling Days =
MEDIAN('vw_Assignee_Workload'[handling_days])
```

```DAX
Max Resolution Days =
MAX(vw_Ticket_Resolution[resolution_days])
```

```DAX
Completion Remaining % =
1 - [Completion %]
```

---

## 9. Time Intelligence

The model uses the `Calendar` table for time-based analysis.

Key measures include:

- `Previous Month Tickets`
- `Previous Month Active Tickets`
- `Previous Month Attention Tickets`
- `Previous Month Completion %`
- `Previous Month Handling (Days)`
- `Previous Month Resolution`
- `Previous Month Waiting (Days)`
- `Previous Month Waiting Monitoring`
- `Latest Month Ticket Change %`
- `Latest Month Active Change %`
- `Latest Month Attention Change %`
- `Latest Month Completion Change`
- `Latest Month Waiting Monitoring Change %`
- `Latest 12 Months Flag`

These measures provide the time context required for monthly comparisons and dashboard trends.

---

## 10. Dashboard Display Measures

Additional DAX measures control the presentation of analytical results. These include:

- `Resolution Trend Display`
- `Resolution Trend Color`
- `Handling Trend Display`
- `Handling Trend Color`
- `Waiting Trend Display`
- `Waiting Trend Color`
- `Ticket Trend Display`
- `Ticket Trend Arrow`
- `Active Tickets Trend Display`
- `Active Tickets Trend Arrow`
- `Attention Tickets Trend Display`
- `Attention Tickets Trend Arrow`
- `Completion Trend Display`
- `Completion Trend Arrow`
- `Waiting Monitoring Trend Display`
- `Waiting Monitoring Trend Arrow`

These measures convert analytical results into dashboard-friendly indicators such as percentage changes and directional arrows.

They are presentation helpers rather than independent business KPIs.

---

## 11. Dashboard Utility Measures

The model also contains measures supporting dashboard navigation and refresh information:

- `Page Indicator`
- `Page Indicator 2`
- `Page Indicator 3`
- `Page Indicator 4`
- `Last Refresh Footer`

### Last Refresh Footer

Displays the latest refresh timestamp from the refresh metadata.

```DAX
Last Refresh Footer =
"Last Refresh: "
    & FORMAT(
        MAX('Refresh Metadata'[Last Refresh]),
        "dd-MM-yyyy HH\:mm"
    )
```

---

## DAX Design Approach

The project separates the analytical workflow into three layers:

```
Power Query
Data cleaning and transformation
        ↓
DAX Measures
Business calculations, aggregations and time-based analysis
        ↓
Power BI Visuals
KPI cards, charts, trends and dashboard presentation
```

This separation keeps data preparation, analytical logic and visual presentation distinct.

---

## Summary

The DAX layer supports the three primary operational KPIs:

- **Resolution Performance**
- **Handling Efficiency**
- **Waiting Bottleneck**

It also provides supporting calculations for ticket volume, workflow states, assignee workload, statistical analysis and month-over-month trends.

Presentation-specific measures are kept separate from the core analytical measures so that the underlying business calculations remain clear.
