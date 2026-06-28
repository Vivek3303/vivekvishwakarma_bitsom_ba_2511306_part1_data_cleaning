# Data Cleaning Log

## Issues Found
- **Text Inconsistencies:** Multiple categorical fields (Region, Segment, Status) contained leading/trailing spaces and inconsistent capitalization.
- **Date Formatting Conflicts:** `order_date` and `ship_date` were a hostile mix of US formats (MM/DD/YYYY), UK/Indian formats (DD-MM-YYYY), and dead text strings (e.g., "21 Jul 2024").
- **Duplicate Records:** The raw dataset contained 933 rows, including exact system clones and conflicting order IDs.
- **Calculation Errors:** Initial margin calculations triggered `#VALUE!` errors when referencing text-based data quality flags. Negative profit margins were observed due to aggressive discounting.

## Cleaning Actions Performed
1. **Text Standardization:** Applied `TRIM()` and `PROPER()` to standardize all string fields.
2. **Date Normalization (Double-Sweep Protocol):** Used Excel's Text-to-Columns engine in two passes (MDY first, then DMY) to force all stubborn text strings into underlying mathematical date values, followed by a custom `YYYY-MM-DD` visual format lock.
3. **Duplicate Handling:** - Purged exactly 20 exact duplicate rows using the Remove Duplicates function (reducing the dataset from 933 to 913 rows).
   - Used `=COUNTIF()` logic to isolate the remaining duplicate `order_id` values. Since exact clones were already purged, these were identified as conflicting records and flagged.
4. **Error Handling in Calculated Columns:** - Wrapped the `profit_margin` calculation in an `=IFERROR([Profit]/[Sales], "Review")` function to gracefully handle text flags without breaking the spreadsheet.

## Business Rules Applied
- Null or blank values in `region` and `ship_mode` were overwritten with "Unknown".
- Null discounts were converted to `0`.
- Orders with negative profit margins were retained, as the underlying math (Cost > Sales) accurately reflects a financial loss taken due to high discounts.
- Non-completed orders (Cancelled/Failed) were retained in the raw log but filtered out of final completed-sales summaries.

## Assumptions & Limitations
- **Assumptions:** Missing discounts indicate full-price sales. Unit price reflects the pre-discount rate. 
- **Limitations:** Conflicting duplicate order IDs could not be resolved without access to the source CRM database; they were flagged as "Duplicate ID - Review" to prevent data loss while alerting stakeholders.
