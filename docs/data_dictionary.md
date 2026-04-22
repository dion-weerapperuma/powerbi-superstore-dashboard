# Data Dictionary – superstore_2021_2024.csv

**Rows:** 1,978 | **Orders:** 800 | **Date Range:** 2021-01-01 to 2024-12-31

---

| Column | Data Type | Example | Description |
|---|---|---|---|
| Row ID | Integer | 1 | Unique row identifier |
| Order ID | Text | CA-2021-10000 | Unique order identifier. Format: CA-YYYY-NNNNN |
| Order Date | Date | 2022-03-15 | Date the order was placed |
| Ship Date | Date | 2022-03-19 | Date the order was shipped |
| Ship Mode | Text | Standard Class | Shipping method selected by customer |
| Customer ID | Text | CG-12345 | Unique customer identifier |
| Customer Name | Text | James Smith | Customer full name |
| Segment | Text | Consumer | Business segment: Consumer, Corporate, Home Office |
| City | Text | San Francisco | City of delivery |
| State | Text | California | US state of delivery |
| Postal Code | Text | 94105 | 5-digit US postal code |
| Region | Text | West | US region: West, East, Central, South |
| Product ID | Text | PRD-123456 | Unique product identifier |
| Category | Text | Technology | High-level product category |
| Sub-Category | Text | Phones | Product sub-category |
| Product Name | Text | Apple iPhone 14 | Full product name |
| Sales | Decimal | 899.00 | Total line-item sales revenue ($) |
| Quantity | Integer | 2 | Number of units ordered |
| Discount | Decimal | 0.20 | Discount applied (0.0 = no discount, 0.4 = 40% off) |
| Profit | Decimal | 143.84 | Net profit for this line item ($) — can be negative |
| Shipping Cost | Decimal | 12.50 | Shipping cost for this line item ($) |

---

## Categorical Values

### Ship Mode
| Value | Approx Lead Time |
|---|---|
| Same Day | 1 day |
| First Class | 2–3 days |
| Second Class | 4–5 days |
| Standard Class | 6–7 days |

### Segment
| Value | Description |
|---|---|
| Consumer | Individual shoppers |
| Corporate | Business accounts |
| Home Office | Small home-based businesses |

### Category & Sub-Categories
| Category | Sub-Categories |
|---|---|
| Technology | Phones, Computers, Accessories, Copiers |
| Furniture | Chairs, Tables, Bookcases, Furnishings |
| Office Supplies | Labels, Storage, Art, Binders, Envelopes, Fasteners, Paper |

### Region & States
| Region | States |
|---|---|
| West | California, Washington, Oregon, Nevada, Arizona |
| East | New York, Pennsylvania, Virginia, Florida, Massachusetts |
| Central | Texas, Illinois, Ohio, Michigan, Minnesota |
| South | Georgia, North Carolina, Tennessee, Alabama, Louisiana |

---

## Notes

- **Negative Profit** values are expected for some Furniture line items (high shipping cost, low margin) and heavily discounted orders (Discount ≥ 0.3).
- **Discount** is stored as a decimal proportion (0.20 = 20%), not a percentage. In Power BI, format this column as a percentage.
- **Sales** represents post-discount revenue: `Sales = Unit Price × Quantity × (1 − Discount)`
- **Profit** is calculated as `Sales × Profit Margin` where margins vary by category (Technology: 8–22%, Furniture: −5–15%, Office Supplies: 10–35%).
