# Blinkit Grocery Sales Analysis

An end-to-end data analysis project on Blinkit grocery sales data using **SQL (PostgreSQL)**, **Python (Pandas, Matplotlib, Seaborn)** and **Power BI**. The goal was to analyse sales performance, customer ratings and outlet performance, and present the results in an interactive dashboard.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| PostgreSQL | Data storage, cleaning and KPI queries |
| Python (Jupyter Notebook) | Data cleaning, exploration and visualisation |
| Power BI | Interactive dashboard (data imported directly from PostgreSQL) |

---

## Dataset

- **File:** `Data/BlinkIT Grocery Data.csv`
- **Size:** 8,523 rows × 12 columns
- **Columns:** Item Fat Content, Item Identifier, Item Type, Outlet Establishment Year, Outlet Identifier, Outlet Location Type, Outlet Size, Outlet Type, Item Visibility, Item Weight, Total Sales, Rating

---

## Project Structure

```
BlinkitDataAnalytics/
├── Data/
│   └── BlinkIT Grocery Data.csv           # Raw dataset
├── Images/                                # Icons used in the Power BI dashboard
├── Blinkit_Business_Requirements.pdf      # KPIs and business questions
├── Blinkit_Grocery_SQL_Analysis.docx      # All SQL queries with explanations
├── blinkit_analysis.ipynb                 # Python analysis notebook
├── powerBI_Analysis.pbix                  # Power BI dashboard
└── README.md
```

---

## Business Requirements

**KPIs**
- Total Sales
- Average Sales
- Number of Items
- Average Rating

**Analysis required**
1. Total Sales by Fat Content
2. Total Sales by Item Type
3. Fat Content by Outlet Location (Total Sales)
4. Total Sales by Outlet Establishment Year
5. Percentage of Sales by Outlet Size
6. Sales by Outlet Location
7. All Metrics by Outlet Type

---

## Analysis Steps

### 1. Defining Requirements
- Wrote down the business questions and KPIs (see `Blinkit_Business_Requirements.pdf`).

### 2. SQL Analysis (PostgreSQL)
- Imported the CSV into a PostgreSQL table `blinkit_data`.
- **Explored data:** previewed rows, counted records, checked distinct values.
- **Cleaned data:** standardised `item_fat_content` values (`LF`, `low fat` → `Low Fat`; `reg` → `Regular`) using `UPDATE` with `CASE`, `LOWER()` and `BTRIM()`.
- **Calculated KPIs:** Total Sales (in millions), Average Sales, Number of Items, Average Rating.
- **Product analysis:** sales by fat content, sales by item type, top 5 item types.
- **Outlet analysis:**
  - Fat content by outlet location (pivot using `SUM() FILTER (WHERE ...)` and `COALESCE`)
  - Sales by outlet establishment year
  - % of sales by outlet size (window function `SUM(SUM(...)) OVER ()`)
  - Sales by outlet location
  - All metrics by outlet type
- All queries are documented in `Blinkit_Grocery_SQL_Analysis.docx`.

### 3. Python Analysis (Jupyter Notebook)
- Loaded the data with Pandas and checked shape, columns, data types and head/tail.
- Cleaned the `Item Fat Content` column (5 inconsistent labels → 2 categories).
- Calculated the four KPIs.
- Created charts with Matplotlib and Seaborn:
  - Pie chart – Sales by Fat Content
  - Bar chart – Total Sales by Item Type
  - Grouped bar chart – Fat Content by Outlet Tier
  - Line chart – Sales by Outlet Establishment Year
  - Pie chart – Sales by Outlet Size
  - Horizontal bar chart – Sales by Outlet Location

### 4. Power BI Dashboard
- Connected Power BI directly to the **PostgreSQL database** to import the cleaned data.
- Created DAX measures for the KPIs (Total Sales, Average Sales, Number of Items, Average Rating).
- Built visuals for all the business requirements: KPI cards, donut/pie charts, bar charts, line chart, matrix table for outlet type.
- Added slicers/filters (e.g. outlet location, outlet size, item type) for interactive analysis.
- File: `powerBI_Analysis.pbix`

<!-- Add a dashboard screenshot here, e.g.:
![Dashboard](Images/dashboard.png)
-->

---

## Key Results

| KPI | Value |
|-----|-------|
| Total Sales | $1.20M |
| Average Sales | $141 |
| Number of Items | 8,523 |
| Average Rating | 3.97 |

## Key Insights

- **Low Fat** products bring in **64.6%** of total sales, compared with 35.4% for Regular.
- **Fruits & Vegetables** and **Snack Foods** are the top-selling categories, followed by Household, Frozen Foods and Dairy.
- **Tier 3** outlets give the highest sales (39.3%), followed by Tier 2 (32.7%) and Tier 1 (28.0%).
- **Medium** size outlets contribute the most sales (42.3%), followed by Small (37.0%) and High (20.7%).
- **Supermarket Type1** is the biggest outlet type, with **65.5%** of total sales.
- Outlets established in **1998** have the highest sales (~$205K). Sales for other years stay fairly stable at ~$130K, except 2011 (~$78K).
- `Item Weight` has 1,463 missing values. It was not used in the KPIs, so these rows were kept.

---

## How to Run

1. **SQL:** Create a PostgreSQL table `blinkit_data`, import the CSV, then run the queries from `Blinkit_Grocery_SQL_Analysis.docx`.
2. **Python:** Install `pandas numpy matplotlib seaborn` and open `blinkit_analysis.ipynb` in Jupyter / VS Code.
3. **Power BI:** Open `powerBI_Analysis.pbix` in Power BI Desktop. Update the PostgreSQL connection settings if you need to refresh the data.
