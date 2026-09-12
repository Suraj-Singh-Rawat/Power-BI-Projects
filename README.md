# Fraud Detection Analytics Dashboard

Power BI dashboard analyzing 60,000 global transactions (10 countries, full year 2024) to identify fraud risk patterns across geography, channel, device, and customer behavior.

Overall fraud rate: 3.36% | Highest-risk channel: Wire Transfer (9.0%) | New device fraud rate: 4.89% vs 3.23% known devices

DATA MODEL
Star schema built in Power BI (Power Query + DAX): Fact_Transactions joined to Dim_Date, Dim_Geography, Dim_Device, Dim_Payment_Method, Dim_Transaction_Type, Dim_Customer_Segment, Dim_Hour_Bucket.

KEY FEATURES
- Dynamic metric selector using a disconnected table + SWITCH measure (no relationship to the model)
- Conditional-formatted risk matrices (Country x Transaction Type)
- 4 pages: Overview, Geography, Risk Deep-Dive, CEO Summary
- Bookmark-driven navigation and All Transactions / Fraud Only toggle
- Role-based view design (Team / Manager / CEO)

TOOLS
Power BI Desktop (Power Query, DAX, bookmarks, custom buttons, conditional formatting), MS Excel
