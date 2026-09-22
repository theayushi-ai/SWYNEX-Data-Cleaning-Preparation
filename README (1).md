# SWYNEX-Data-Cleaning-Preparation

Task 1 of the SWYNEX Data Analyst Internship: clean and prepare a raw dataset for analysis.

## Dataset
- **Name:** Retail Store Sales (dirty dataset)
- **Source:** Kaggle - <paste dataset link here>
- **Size:** 12,575 rows x 11 columns
- **Columns:** Transaction ID, Customer ID, Category, Item, Price Per Unit, Quantity, Total Spent, Payment Method, Location, Transaction Date, Discount Applied

## Tools Used
Python, pandas, NumPy (Google Colab)

## Issues Found

| Issue | Details |
|---|---|
| Missing values | Item: 1,213 / Price Per Unit: 609 / Quantity: 604 / Total Spent: 604 / Discount Applied: 4,199 |
| Duplicate records | 0 duplicate rows, 0 duplicate Transaction IDs |
| Incorrect data types | `Transaction Date` stored as text; `Discount Applied` stored as text |
| Inconsistent values | No spelling/case issues found in Category, Payment Method or Location |
| Numeric consistency | `Total Spent = Price Per Unit x Quantity` held true for every complete row (0 mismatches); no negative or out-of-range values |

## Cleaning Steps

1. **Standardised column names** to lowercase with underscores (e.g. `Price Per Unit` -> `price_per_unit`).
2. **Checked for duplicates** using `drop_duplicates()` on full rows and on `transaction_id`. None were found, so no rows were removed.
3. **Fixed data types:** `transaction_date` -> datetime, `discount_applied` -> boolean, numeric columns -> float.
4. **Filled missing values using the business rule** `Total Spent = Price x Quantity`:
   - missing `total_spent` = `price_per_unit` x `quantity`
   - missing `price_per_unit` = `total_spent` / `quantity`
   - missing `quantity` = `total_spent` / `price_per_unit`
5. **Item:** missing values replaced with `"Unknown"`, because the item name cannot be derived from other columns.
6. **Dropped 604 rows** where both `quantity` and `total_spent` were missing, since they cannot be recovered without guessing (about 4.8% of the data).
7. **Discount Applied** was left as missing (3,988 values) on purpose, because it is not possible to know whether a discount was applied.
8. Converted `quantity` to integer and saved the result as `cleaned_data.csv`.

## Before vs After

| Metric | Before | After |
|---|---|---|
| Rows | 12,575 | 11,971 |
| Columns | 11 | 11 |
| Total missing values | 7,229 | 3,988 (all in `discount_applied`) |
| Duplicate rows | 0 | 0 |
| `transaction_date` type | object | datetime64 |
| `discount_applied` type | object | boolean |

## Files in this Repository
- `retail_store_sales.csv` - original raw dataset
- `cleaned_data.csv` - cleaned dataset
- `Retail_store_sales.ipynb` - cleaning notebook (step-by-step code)
- `README.md` - this file
