# Adidas Sales Dashboard (Power BI)

An interactive 4-page Power BI dashboard that analyzes Adidas US sales for 2020 and 2021. The goal is to find where the business makes money, not only where it sells the most.

## Key numbers

| Metric | Value |
|---|---|
| Total sales | $899.9M |
| Operating profit | $332.1M |
| Operating margin | 36.9% |
| Units sold | about 2M |
| Period | January 2020 to December 2021 |

## Dashboard preview

| Overview | Product analysis |
|---|---|
| ![Overview](<Assets/Screenshot 2026-10-08 200657.png>) | ![Product analysis](<Assets/Screenshot 2026-10-08 200707.png>) |

| Retailer and channel | Insights and recommendations |
|---|---|
| ![Retailer and channel](<Assets/Screenshot 2026-10-08 200718.png>) | ![Insights](<Assets/Screenshot 2026-10-08 200727.png>) |

## Tools

- Excel (source data)
- Power Query (data cleaning, M language)
- Power BI Desktop (data model, visuals)
- DAX (measures, date table)

## Data model

A star schema:

```
Dim_Date ──(1 → *)── Fact_Sales ──(* ← 1)── Dim_Retailer
```

| Table | Role | Key |
|---|---|---|
| Fact_Sales | One row per sale (sales, profit, units, product, location, channel) | Invoice Date, Retailer |
| Dim_Retailer | Unique retailers (6) | Retailer |
| Dim_Date | Continuous calendar table, marked as date table | Date |
| _Measures | Holds all DAX measures | none |

`Retailer ID` repeats across several retailers in the source, so the model joins on the retailer name instead.

## Data preparation (Power Query)

1. Removed the first 4 header rows and promoted the real header row.
2. Set data types (dates, decimals, percentage, text).
3. Added calculated columns: Gender, Category, Type, Margin Category, Price Tier, Order Size.
4. Moved time columns to a separate date table and removed the calculated numeric columns from the fact table (they are measures instead).
5. Checked for duplicates and trimmed text so relationships match.

## Main measures (DAX)

```
Total Sales     = SUM(Fact_Sales[Total Sales])
Total Profit    = SUM(Fact_Sales[Operating Profit])
Units Sold      = SUM(Fact_Sales[Units Sold])
Margin %        = DIVIDE([Total Profit], [Total Sales])
Total Costs     = [Total Sales] - [Total Profit]
Cost per Unit   = DIVIDE([Total Costs], [Units Sold])
Profit per Unit = DIVIDE([Total Profit], [Units Sold])
Avg Price       = DIVIDE([Total Sales], [Units Sold])
```

Margin is calculated as total profit divided by total sales, not as an average of row-level margins.

## Dashboard pages

| Page | What it answers |
|---|---|
| Overview | How big is the business, how does it change over time, and which regions and channels lead? |
| Product analysis | Which products are stars (high sales and high margin) and which need review? Includes a sales vs margin scatter. |
| Retailer and channel | Who are the top retailers, and which sales method earns the best margin? |
| Insights and recommendations | Written findings, recommendations, and data notes. |

Interactivity: slicers (year, region, product, and more), cross-filtering between visuals, page navigation buttons, and synced Year and Region slicers across pages.

## Main findings

- West leads regional sales, while South earns a 42.3% margin versus 36.5% in Northeast, which makes South the best expansion opportunity.
- In-store is the largest channel (39.6% of sales), then Outlet (32.9%) and Online (27.5%).
- Sales peak in July and August, with a low in March and another dip in October.
- Women's Apparel has some of the strongest margins; Athletic footwear sells the least.

## Recommendations

1. Expand in South.
2. Prepare stock and campaigns before the summer peak, and run promotions in the slow months.
3. Support Women's Apparel with marketing.
4. Review pricing and costs for Athletic footwear.
5. Grow Online carefully and check its margin first.

## Data notes and limitations

- Costs are derived (Total Sales minus Operating Profit); the source has no cost column.
- In the source, Total Sales is about 10 times Price per Unit multiplied by Units Sold. Per-unit measures (Avg Price, Cost per Unit, Profit per Unit) are therefore useful for comparison only, not as exact prices.
- Retailer ID is not unique per retailer, so it is not used as a key.

## How to open the project

1. Install Power BI Desktop.
2. Open `adidas.pbix`.
3. If Power BI asks for the data source, point it to `Assets/Adidas US Sales Datasets.xlsx` (**Home > Transform data > Data source settings**), then click **Refresh**.

## Project structure

```
Assets/
  Adidas US Sales Datasets.xlsx    source data
  Screenshot 2026-10-08 200657.png dashboard screenshots (4 pages)
  Screenshot 2026-10-08 200707.png
  Screenshot 2026-10-08 200718.png
  Screenshot 2026-10-08 200727.png
adidas.pbix                        Power BI report
README.md
```

## Author

Ahmed Elsayed  | [LinkedIn](https://www.linkedin.com/in/ahmedel-saied/)   | [GitHub](https://github.com/ahmedelsaiedabdelrhim-prog)
