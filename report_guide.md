# Report Guide – Step-by-Step Power BI Setup

This guide walks you through building the Superstore Dashboard from scratch in Power BI Desktop. Estimated time: 45–60 minutes.

---

## Prerequisites

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) installed (free)
- `data/superstore_2021_2024.csv` available locally

---

## Step 1: Load the Data

1. Open Power BI Desktop → **Home → Get Data → Text/CSV**
2. Browse to `data/superstore_2021_2024.csv` and click **Open**
3. In the preview dialog, verify the delimiter is **Comma** and data looks correct
4. Click **Transform Data** to open Power Query Editor

---

## Step 2: Power Query Transformations

Apply these transformations in Power Query Editor:

### Column Types
Set the following column types explicitly:

| Column | Type |
|---|---|
| Row ID | Whole Number |
| Order Date | Date |
| Ship Date | Date |
| Sales | Decimal Number |
| Quantity | Whole Number |
| Discount | Decimal Number |
| Profit | Decimal Number |
| Shipping Cost | Decimal Number |
| Postal Code | Text (important — prevents stripping leading zeros) |

### Add a "Days to Ship" Column
1. **Add Column → Custom Column**
2. Name: `Days to Ship`
3. Formula: `= Duration.Days([Ship Date] - [Order Date])`

### Add a "Year" Column (optional, for quick slicers)
1. **Add Column → Date → Year**
2. Select `Order Date` column first

### Rename the Query
- In the Queries pane on the left, rename the query from `superstore_2021_2024` to `Orders`

Click **Close & Apply**.

---

## Step 3: Create the Date Dimension Table

1. **Home → Transform Data → New Source → Blank Query**
2. Open **Advanced Editor** and paste the M Query from [`dax/measures.md`](../dax/measures.md)
3. Name the query `DateTable`
4. Click **Close & Apply**
5. In the Report view, go to **Table Tools → Mark as Date Table** → select `Date` column

---

## Step 4: Create the Relationship

1. Go to **Model view** (left sidebar)
2. Drag `DateTable[Date]` onto `Orders[Order Date]`
3. Confirm the relationship: Many-to-One, Single direction (DateTable → Orders)

---

## Step 5: Create DAX Measures

1. Select the `Orders` table in the Fields pane
2. **Table Tools → New Measure**
3. Copy-paste each measure from [`dax/measures.md`](../dax/measures.md)

Recommended creation order:
1. `Total Sales`
2. `Total Orders`
3. `Total Profit`
4. `Profit Margin %`
5. `YoY Sales Growth`
6. `MTD Sales` / `YTD Sales`
7. `Avg Order Value`
8. `Total Customers`

---

## Step 6: Build Report Pages

### Page 1 – Executive Summary

| Visual | Fields |
|---|---|
| Card | Total Sales |
| Card | Total Profit |
| Card | Total Orders |
| Card | Avg Order Value |
| Line Chart | Axis: DateTable[Year-Month] / Values: Total Sales, Total Profit |
| Bar Chart | Axis: Orders[Region] / Values: Total Sales |
| Bar Chart | Axis: Orders[Sub-Category] / Values: Total Sales (Top N = 5 filter) |

Add a **Year slicer** using `DateTable[Year]`.

### Page 2 – Sales Deep Dive

| Visual | Fields |
|---|---|
| Bar Chart | Axis: Orders[Category] / Values: Total Sales, YoY Sales Growth |
| Scatter Plot | X: Avg Discount / Y: Profit Margin % / Size: Total Sales / Legend: Category |
| Donut Chart | Legend: Orders[Ship Mode] / Values: Total Sales |
| Matrix | Rows: DateTable[Month Name] / Columns: DateTable[Year] / Values: Total Orders |

### Page 3 – Customer & Segment

| Visual | Fields |
|---|---|
| Donut Chart | Legend: Orders[Segment] / Values: Total Sales |
| Bar Chart | Axis: Orders[Customer Name] / Values: Total Sales (Top N = 10) |
| Matrix | Rows: Orders[Segment] / Columns: Orders[Category] / Values: Total Sales |
| Card | Total Customers |
| Card | Sales per Customer |

### Page 4 – Regional Performance

| Visual | Fields |
|---|---|
| Map (Filled Map) | Location: Orders[State] / Values: Total Sales |
| Matrix | Rows: Orders[Region] / Columns: Orders[Segment] / Values: Total Sales |
| Bar Chart | Axis: Orders[Region] / Values: Profit Margin % |
| Card | Top Region (using the DAX measure) |

---

## Step 7: Formatting Tips

- Set the **report theme** to a custom colour palette (recommended: dark blue / teal accent)
- Use **consistent title fonts**: Segoe UI, size 14 for page titles, 11 for visual titles
- Enable **cross-filtering** between all visuals on a page (default in Power BI)
- Add a **navigation bar** on the left with page buttons for professional look
- Use **conditional formatting** on the Profit column in matrices (red for negative)

---

## Step 8: Export as .pbit Template

1. **File → Export → Power BI Template**
2. Name it `Superstore_Dashboard.pbit`
3. Add description: `"Global Superstore Sales Dashboard 2021–2024 – Kaggle-inspired dataset"`
4. Save to the project root folder

This allows others to clone your repo and connect to their own data path.

---

## Troubleshooting

| Issue | Fix |
|---|---|
| Date table not working | Make sure DateTable is marked as a Date Table in Table Tools |
| YoY measure returns blank | Ensure the relationship between Orders and DateTable is active |
| Map not showing states | Check that State column has valid US state names (not abbreviations) |
| Negative profit not red | Apply conditional formatting: Format pane → Font Color → Rules |
