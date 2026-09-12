## Fraud Detection Analytics Dashboard (PROJECT 1)

Power BI dashboard analyzing 60,000 global transactions (10 countries, full year 2024) to identify fraud risk patterns across geography, channel, device, and customer behavior.

DATA MODEL
- Star schema built in Power BI (Power Query + DAX): Fact_Transactions joined to Dim_Date, Dim_Geography, Dim_Device, Dim_Payment_Method, Dim_Transaction_Type, Dim_Customer_Segment, Dim_Hour_Bucket.

KEY FEATURES
- Dynamic metric selector using a disconnected table + SWITCH measure (no relationship to the model)
- Conditional-formatted risk matrices (Country x Transaction Type)
- 4 pages: Overview, Geography, Risk Deep-Dive, CEO Summary
- Bookmark-driven navigation and All Transactions / Fraud Only toggle
- Role-based view design (Team / Manager / CEO)

KEY FINDINGS
- Wire Transfer is the highest-risk channel: 9.0% fraud rate, roughly 3x higher than POS Payment (2.8%)
- New/unrecognized devices carry meaningfully higher risk: 4.89% fraud rate vs 3.23% for known devices
- Risk factors compound: combining a high-risk channel (Wire Transfer) with a high-risk country pushes fraud rate above 13% in some cells, vs the 3.36% overall baseline
- Monthly fraud rate is fairly stable year-round (3.1%-3.8% range), suggesting no strong seasonal fraud pattern in this data - risk is driven more by channel/device/behaviour than time of year

TOOLS
 - Power BI Desktop (Power Query, DAX, bookmarks, custom buttons, conditional formatting)
 - MS Excel


## IT Support Tickets — Power BI Analytics (Project 2)

An end-to-end Power BI project on a 100,000-row IT support ticket dataset — data modeling, DAX, and a 4-page interactive report with bookmarks.

**Data source**: SQL Server export.

### Data model

Star schema — `Fact_Tickets` + 12 dimensions (Date, Priority, Status, SLA, Sentiment, Channel, Platform, Region, Segment, Issue Type, Product Area, Customer).

- ~40% of tickets have no resolution time (still open) — left null, never zero-filled
- `customer_id` isn't a stable profile in this data, so `Dim_Customer` holds no attributes beyond the ID
- `csat_score = 0` means "no response," not "worst score" — tracked separately via `has_csat_response`
- No SLA target-hours field exists, so "compliance" isn't calculable — median resolution time by SLA plan is used as a labeled proxy instead
- Priority, sentiment, and SLA plan are ordinal — each has a rank column, sorted via Power BI's "Sort by column" so charts read Low→Urgent instead of A→Z

---

### Report pages

1. **Overview** — KPIs, monthly volume by priority, segment/region breakdown. Slicers: date, region, SLA plan.
2. **Operations** — resolution time by priority, volume by priority, status mix, SLA/issue-type breakdowns.
3. **Customer Experience** — CSAT distribution, sentiment mix, sentiment by channel, resolution-vs-CSAT scatter.
4. **Channel & Platform** — platform × region heatmap, channel/platform volume, attachment & reopen rate by channel.


### Key measures

```dax
Total Tickets = COUNTROWS(Fact_Tickets)
Resolution Rate = DIVIDE([Resolved Tickets], [Total Tickets])
Avg Resolution Time (Hrs) = CALCULATE(AVERAGE(Fact_Tickets[ResolutionTimeHours]), Fact_Tickets[is_resolved] = 1)
Avg CSAT (Responses Only) = CALCULATE(AVERAGE(Fact_Tickets[CsatScore]), Fact_Tickets[has_csat_response] = 1)
```
Full set is in the `_Measures` table inside the `.pbix`.

---

## Key findings

- **Priority drives speed**: avg resolution drops from ~40 hrs (low) to ~4 hrs (urgent) — a real, consistent pattern.
- Most other cuts (channel, platform, region, product area) come out flat — expected given the source data's structure, and exactly the kind of chart that would catch a real problem in production data.
- ~30% of tickets never got a CSAT score — treating that as "0 = worst" would understate satisfaction rather than reflect it.

---

## Tools

SQL Server · Power BI Desktop(DAX, Power Query, bookmarks, custom buttons, conditional formatting)
