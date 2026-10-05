# Investment Portfolio Tracker

An interactive multi-investor portfolio dashboard built in **Power BI**, using a star-schema data model, DAX measures and cross-filtering slicers.

> **Performance overview: 2023 – 2025** · 1 page · 5 KPI cards · 5 charts/tables · 4 slicers

---

<img width="1203" height="681" alt="image" src="https://github.com/user-attachments/assets/3deb5a12-7d05-42f9-8651-eab83de60adf" />

## Business Questions Answered

A portfolio analyst asks the same few questions every day:

1. How much have we invested?
2. What is it worth now?
3. Which assets are working and which are not?
4. How does this differ by investor, asset type, month and risk profile?

This dashboard answers all four on a single page, and every number updates instantly with each slicer selection.

---

## Dashboard Layout

| Section | Visual | Purpose |
|---|---|---|
| Header | Title + 4 slicers | Investor Name · Month · Asset Type · Risk Profile |
| KPI row | 5 cards | Total Investment · Total Current Value · Total Return % · Top Performing Asset · Total Return |
| Middle left | Pie chart | Current value by asset type |
| Middle centre | Donut chart | Original investment by asset type |
| Middle right | Area/line chart | Current value by month |
| Bottom left | Column chart | Total return by asset type |
| Bottom right | Table | Position-level detail: asset, invested value, quantity, current value, return |

**Design:** monochrome black / grey / white theme with italic serif headings, built from a custom Power BI theme file (`portfolio-tracker-powerbi-theme.json`).

---

## Headline Numbers

| Metric | Value |
|---|---|
| Total invested | **₹274.55M** (shown as 275M) |
| Current value | **₹301.78M** |
| Total return | **₹27.23M** |
| Return % | **9.92%** |
| Total units held | 100,128 |

---

## Data Model (Star Schema)

```
        Dim_Investor
             │
Dim_Date ── Fact_Investments ── Dim_Asset
```

| Table | Type | Key fields |
|---|---|---|
| Fact_Investments | Fact | Invested_Value, Current_Value, Quantity, Return_% |
| Dim_Asset | Dimension | Asset_ID, Asset_Name, Asset_Type |
| Dim_Investor | Dimension | Investor_ID, Investor_Name, Risk_Profile |
| Dim_Date | Dimension | Date, Month, Month_Year, Quarter, Year |

The dataset was designed from scratch in Excel. Transactional data is kept separate from descriptive attributes, which is what lets slicers cross-filter without double counting.

---

- **DIVIDE** instead of `/`: returns blank instead of an error when a slicer leaves no data.
- **SUMX** instead of `SUM`: explicit row-by-row iteration, reliable when the fact table has several rows per asset or investor.
- **TOPN + MAXX** for the top performer: returns the asset *name* (a text result) and recalculates under any filter context.

---

## Key Insights

### 1. Portfolio is profitable, but modestly
The portfolio gained ₹27.23M on ₹274.55M invested, a **9.92% return**. If that spans the full 2023–2025 window, it is roughly **3.2% a year**, so it should be benchmarked against a fixed deposit or index fund before being called a success.

### 2. Gold ETF is the standout among visible holdings

| Position | Invested | Return |
|---|---|---|
| Gold ETF | ₹25.6M | **+10.1%** |
| Bitcoin | ₹29.9M | +9.9% |
| LIC Insurance | ₹29.1M | +9.5% |
| Axis Bluechip Fund | ₹23.3M | +7.9% |
| Reliance | ₹31.0M | +6.9% |

- Gold ETF earned the most per rupee with the least risk, which makes it the strongest candidate for a larger allocation.
- **Reliance is the largest position (~11% of money invested) and the weakest performer**, so the most capital sits in the laggard.
- Returns across very different asset types fall in a narrow 7–10% band. The dataset does not strongly separate high-risk from low-risk assets, so risk-profile comparisons will show little difference.

### 3. Positions are well spread
Each position is roughly 8–11% of the money invested, so there is no single dangerous concentration.

### 4. Seasonality in monthly value
- Monthly current value ranges from **19.7M (March, low)** to **29.9M (October, peak)**.
- The second half of the year is about **15% higher** than the first half (161.3M vs 140.4M).
- Q1 is the weakest quarter (66.9M) and Q3 the strongest (81.6M).
- December drops about **20%** from November (27.7M → 22.2M), which is worth checking against the underlying records.

---


## Skills Demonstrated

- Star-schema modelling with defined relationships, not a flat-file import
- DAX with filter-context awareness (SUMX, DIVIDE, TOPN, MAXX)
- Cross-filtering slicers controlling every visual at once
- Visual choice driven by analytical purpose
- Single-page layout flowing from summary (KPIs) to detail (table)
- Custom Power BI theme (JSON) for a consistent monochrome style
- Pie vs donut side by side to show investment vs current-value allocation (portfolio drift)

---

## File Structure

```
Investment-Portfolio-Tracker
├── PORTFOLIO_TRACKER.pbix                  # Power BI dashboard
├── Portfolio_Dataset.xlsx                  # Source data (star-schema tables)
├── portfolio-tracker-powerbi-theme.json    # Custom Power BI theme
└── README.md
```


Author:
Azli Kham
