# Data Quality Report — Books to Scrape

## 1. Number of Rows Produced
- **Total rows scraped:** 1000
- **Final clean rows in `books.csv`:** 1000 (expected: 1,000 across 50 catalogue pages × 20 books/page)

## 2. Null Rate by Column
| Column | Null Count | Null Rate |
| :--- | :---: | :---: |
| `title` | 0 | 0.00% |
| `category` | 0 | 0.00% |
| `price_gbp` | 0 | 0.00% |
| `rating` | 0 | 0.00% |
| `stock_count` | 0 | 0.00% |
| `upc` | 0 | 0.00% |
| `num_reviews` | 0 | 0.00% |
| `product_url` | 0 | 0.00% |

## 3. Dropped Records & Reasons
- **0 rows dropped.** All 1,000 target product pages returned HTTP 200 and parsed cleanly without duplicates or missing core identifiers.

## 4. Validation & Correctness Checks
Automated assertions and spot-checks were run against `books.csv` to verify schema compliance and extraction accuracy:

- **Completeness & Uniqueness:** Confirmed `1000` unique UPCs and `1000` unique URLs (`PASS`).
- **Category Breadcrumb Accuracy:** Verified that `category` reflects the specific genre (e.g., *Poetry*, *Travel*, *Fiction*) from the 3rd breadcrumb link on each book's page rather than the generic `"Books"` parent link (`PASS`).
- **Price Formatting (`price_gbp`):** Confirmed all values are positive numeric floats (`min: £10.00`, `max: £59.99`) with currency symbols stripped (`PASS`).
- **Rating Range (`rating`):** Verified all ratings are integers strictly within `[1, 5]` (`PASS`).
- **Stock & Reviews (`stock_count`, `num_reviews`):** Confirmed non-negative integer counts across all rows (`PASS`).
- **Manual Spot-Check:** Cross-referenced random rows (including *A Light in the Attic*, UPC `a897fe39b1053632`, `£51.77`, rating `3`, stock `22`, reviews `0`) directly against the live HTML source.
