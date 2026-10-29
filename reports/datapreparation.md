# Data Cleaning and Preprocessing

This report describes how `notebooks/01_data_preparation.ipynb` transforms the June 2026 DB1C Ticket source file into the processed dataset in `data/processed`. The resulting sample is intended for exploratory analysis and regression of ordinary commercial ticket fares.

## Source and memory-efficient inspection

The source is `data/raw/DB1C.TICKET.202606.16SEP2026.csv`, a 13,822,006-row, approximately 2.52 GB itinerary-level file. The notebook first inspects a ten-row preview and the first 100,000-row chunk to review field names, data types, missingness, category counts, fare distributions, and variable relationships. The first chunk is used only for diagnostics; it is not used as the analysis sample because its position in the file does not make it representative.

The raw file is read in 200,000-row chunks. During sampling, the notebook reads only 19 fields needed for the analysis: itinerary ID; reporting and scheduled-flight year/month; itinerary characteristics; origin geography; reporting and issuing carriers; purchase window; fare fields; and total distance. This limits memory use while preserving the information needed for cleaning and screening.

## Reproducible sampling

The notebook draws a sample of exactly 100,000 records from the full file. With NumPy's random generator seeded at 42, it assigns each row a random priority as chunks are read, keeps the smallest priorities within and across chunks, and finally retains the 100,000 lowest-priority records. This produces a reproducible, file-wide random sample without loading the full source into memory.

The random sample, rather than the initial inspection chunk, is the basis for all subsequent classification and cleaning. Its 100,000 rows provide the starting point for the prepared dataset.

## Fare quality assessment and classification

The dependent variable is `TotalAmount`. Inspection shows a strongly right-skewed distribution and unusually frequent low values. Instead of removing fares below an arbitrary `TotalAmount` threshold, the notebook uses the fare component excluding tax:

```text
AmountExclTax = round(TotalAmount - TaxAmount, 2)
```

Rounding to cents before classification avoids floating-point subtraction artifacts. Records are then assigned a `FareRecordType`:

- **Missing:** the derived fare component is missing because required fare information is missing.
- **Tax-only:** `AmountExclTax` equals zero. These records have no positive non-tax fare component; common ticket totals include 5.60 and 11.20.
- **Nominal fare:** `AmountExclTax` is greater than zero and no greater than 0.02. These 0.01–0.02 values form a discrete pattern, concentrated among a small number of carriers.
- **Regular positive fare:** `AmountExclTax` exceeds 0.02.

In the random sample, 9,339 records are tax-only, 545 are nominal fare, and six have missing fare information. The main analysis sample keeps only `Regular positive fare` records: 90,086 observations (90.09% of the starting sample). This excludes the identified special fare populations without applying a broad minimum-fare cutoff. The minimum retained non-tax fare is positive. High fare observations are retained; the notebook defers assessment of their influence to later regression diagnostics.

## Variable screening and retained fields

The notebook assesses redundancy, missingness, variation, category size, and suitability for modelling:

- `MilesTraveled` is excluded because it is almost perfectly correlated with `TotalDistance` and is exactly equal to it for most inspected records. `TotalDistance` is retained.
- `TaxAmount` and `ItinYield` are not treated as candidate predictors of `TotalAmount`, since they are fare-related and can have a mechanical relationship with the response. `TaxAmount` is retained in the prepared file for descriptive checks; the derived `AmountExclTax` documents the fare classification.
- `IssuingCarrier` is excluded from the prepared fields because it matches `RpCarrier` for 99.94% of the cleaned sample. `RpCarrier` is retained.
- Constant or near-constant reporting/scheduled-flight time fields are not selected as predictors. `RpYear` and `RpMonth` remain as metadata; scheduled-flight year/month do not appear in the final file.
- `OriginState` is retained as a manageable geographic candidate. `Origin` and `OriginCityMarketID` are also retained for descriptive analysis or sensitivity checks, although their hundreds of categories make them less suitable for the initial broad model.
- Rare `RpCarrier` categories are not combined during data preparation. The original carrier values are preserved so any grouping decision can be made at the modelling stage.
- The candidate predictor set is `TotalDistance`, `Coupons`, `RoundTrip`, `OnLine`, `PurchaseWindowGroup`, `RpCarrier`, `ItinGeoType`, and `OriginState`. This is a screening result, not a formal model-based selection.

The final 16 columns are: `ItinID`, `TotalAmount`, `TotalDistance`, `Coupons`, `RoundTrip`, `OnLine`, `PurchaseWindowGroup`, `RpCarrier`, `ItinGeoType`, `OriginState`, `Origin`, `OriginCityMarketID`, `TaxAmount`, `AmountExclTax`, `RpYear`, and `RpMonth`.

## Integrity checks and output

Before export, `RoundTrip`, `OnLine`, and `OriginCityMarketID` are explicitly stored as integer types. The notebook checks missing values, duplicate itinerary IDs, fully duplicated rows, and ranges of key numeric fields. The resulting 90,086-row, 16-column dataset has no missing values, no duplicate `ItinID` values, no fully duplicated rows, and no negative fare or distance values.

The same prepared table is exported without a pandas index in two formats:

- `data/processed/db1c_ticket_prepared_202606.parquet` — preferred for subsequent notebooks because it is compact and preserves data types reliably.
- `data/processed/db1c_ticket_prepared_202606.csv` — portable text copy for manual inspection and use in other tools.

The cleaning process therefore produces one consistent processed sample for downstream exploratory analysis and regression, while retaining identifiers and selected descriptive fields alongside the response and candidate predictors.
