# Clothing Store: Returns and Discounts Report

An Excel report on product returns and discounts for a (fictional) online clothing store, built for a sales manager.

> **Training project.** The report was created during the **Eesti.ai training course** as a practice exercise in data analysis and report design. The original was written in Estonian; this repository contains the English version. All data is **synthetic sample data** and the figures are illustrative only.

![Report preview](docs/report_preview.png)

## Business questions

1. **Which product categories are returned most?**
2. **Is returning linked to discounts?**
3. **How do the return rate and the average discount develop month by month?**

## Key findings (sample data, 2025-01 – 2026-09)

| Metric | Value |
|---|---|
| Total return rate (returned / sold units) | **14.3%** |
| Average discount (weighted by units sold) | **11.5%** |
| Return rate with no discount | 13.7% |
| Return rate at a 41%+ discount | 16.5% |

- **Highest return rates:** Dresses (20.9%) and Footwear (20.7%), followed by Trousers (17.1%). Underwear & socks has the lowest rate (5.0%).
- **Largest share of all returns:** Footwear (24.4%) and Dresses (23.1%).
- **Discounts and returns:** the return rate is about 13.7–13.8% at discounts up to 20% and jumps to about 16.5% above 20% (21–40% and 41%+ are almost equal).
- **Monthly trend:** the average discount peaked in 2026-01 (22.4%) and 2026-07 (20.8%). The return rate rose in July (17.4%) but not in January (14.6%).
- The link between discounts and returns is a **correlation, not proven causation**: product mix, season and price class are not separated.

## Repository contents

```
├── clothing_store_returns_discounts_report.xlsx   # the report (English)
├── docs/report_preview.png                        # preview of the Report sheet
└── README.md
```

### Workbook sheets

| Sheet | Contents |
|---|---|
| **Report** | 4 key metrics, 3 charts, an auto-updating comment, calculation tables A–D and a data check (from row 56) |
| **Data** | 10,000 synthetic sales rows, 7 columns (`A1:G10001`) |
| **User guide** | How to replace the data, data structure, metric definitions, design notes |

### Data structure (`Data` sheet)

| Column | Type | Description |
|---|---|---|
| `order_id` | text | Unique sales row ID, format `T-NNNNN` |
| `date` | date | Order date, 2025-01-01 to 2026-09-30 |
| `product_category` | text | One of 7 categories: Dresses, Shirts & tops, Trousers, Jackets & coats, Footwear, Accessories, Underwear & socks |
| `quantity` | integer | Units in the sales row, 1–5 |
| `price_eur` | decimal | Unit price in EUR before discount, VAT included |
| `discount_%` | integer | Discount in percent, no `%` sign: 0, 10, 15, 20, 30, 40 or 50 |
| `returned` | text | `Y` if the sales row was returned, otherwise `N` |

## Metrics and method

| Metric | Definition |
|---|---|
| Return rate | Returned units / sold units × 100 (by category, discount level and month) |
| Share of total returns | Category returned units / all returned units × 100 |
| Average discount | Σ(quantity × discount %) / Σ quantity (weighted by units, not price) |
| Return rate by discount level | Groups: 0%, 1–20%, 21–40%, 41%+ |

- All numbers and charts are calculated with **formulas** (`SUMIFS`, `SUMPRODUCT`, `INDEX/MATCH`, `EDATE`, `FIXED`) over named ranges (`d_order_id`, `d_date`, `d_category`, `d_quantity`, `d_price`, `d_discount`, `d_returned`).
- The monthly chart always shows the **last 12 months** in the data.
- A whole sales row counts as returned (all of its units) when `returned = Y`.

### Data quality check

The data contains a few deliberately inconsistent values, and the **data check** (Report, row 93 onwards) reports them:

- **3 rows** with an unknown category (`Puksid` ×2, `Kleidid` ×1).
- **199 rows** where `returned` is neither `Y` nor `N` (for example `yes`, `no`, `true`, `false`, `0`, `1`, `j`, `e`). These rows are counted as not returned.

## Using your own data

1. Open the **Data** sheet, select `A2:G20001` and delete the contents.
2. Paste your data from `A2` in the same column order (*Paste values*).
3. Check that dates are real dates and that quantity, price and discount are numbers.
4. Open **Report** and make sure all rows of the data check show `OK`.
5. Adjust the category names (`B61:B67`) and discount boundaries (`D72:E75`) if they differ, and remove the *SAMPLE DATA* label (`S2`).

The formulas read up to 20,000 data rows; see the **User guide** sheet for how to extend the range.

## Design

Colours and fonts follow the project design guide: primary `#1F4D47`, secondary `#C8553D`, accent `#E0A93B`, text `#2B2B2B` / `#6B6B6B`, background `#F6F1EA`, lines `#D9D2C7`. Headings use Georgia; body text and tables use Calibri.

## Translation notes

- This is a translation of the Estonian original. Sheet names, labels, chart titles, formula texts, column headers and category names are in English. All calculated values match the original.
- Flag values were changed from `jah` / `ei` to `Y` / `N`, and the formulas were updated accordingly.
- The local Power Query connection to a CSV on the author's computer was removed, so the workbook no longer refreshes from an external file.

## Sources

- Report: `riidepood_tagastused_allahindlused_raport.xlsx` (Estonian original, created during the training course)
- Data description: `riidepoe_muugid_metaandmed.csv`
- Key metric definitions: `votmenaitajad_tagastused_allahindlused.xlsx`
- Design guide: `riidepoe_muugid_disainiraamat.pdf`
