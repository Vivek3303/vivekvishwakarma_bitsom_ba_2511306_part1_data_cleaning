# vivekvishwakarma_bitsom_ba_2511306_part1_data_cleaning

# Part 1: Data Cleaning & Pre-processing

## Business Problem Summary
Raw sales data often contains duplicates, typos, and formatting inconsistencies that prevent accurate analysis. This project involved a multi-stage cleaning process to transform unstructured order records into a high-fidelity, production-ready dataset.

## Data Cleaning & Transformation Methodology
* **Standardization:** Corrected systemic typos in `ship_mode` (e.g., fixed "Unkown" to "Unknown").
* **Data Integrity:** Implemented a `data_quality_flag` column to categorize records as "Clean" or "Duplicate ID - Review," allowing analysts to exclude untrustworthy data from high-stakes forecasting.
* **Calculated Metrics:** Engineered key operational KPIs:
    * `profit_margin`: Derived from `profit / sales` to measure efficiency.
    * `shipping_delay_days`: Calculated as `ship_date - order_date` to pinpoint logistical bottlenecks.
* **Formula Stabilization:** Converted all formula-based columns to **Static Values** (Paste Special > Values). This ensures file performance, prevents `#REF!` errors, and ensures portability between Excel and Tableau.
* **Refinement:** Performed a final audit to remove all redundant raw columns, standardizing headers to industry-standard naming conventions (e.g., `sales`, `profit`, `discount`).

## Dataset Description
* **File:** `Cleaned_Data.csv`
* **Integrity Status:** Bulletproof. No remaining formulas or external references. 
* **Scope:** Standardized transactional data optimized for Pivot Table aggregation and Tableau visualization.
