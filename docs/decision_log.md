# Decision Log

Each entry records what I decided, what else I considered, why, and the trade-off.

---

## 1. Main metric is relief rate, not timely response rate
**Date:** 2026-10-13
**Status:** Decided
**Options considered:** timely response rate, relief rate, complaint volume
**Decision:** Use the share of complaints closed with relief as the main outcome metric.
**Why:** 18,218,355 of 18,329,250 complaints (99.4%) were answered on time, so the timely response rate cannot separate good companies from bad ones.
**Trade-off:** "Non-monetary relief" is a broad category, so the metric depends on how companies label their responses.

---

## 2. Exclude unfinished complaints and incomplete recent data from rates and trends
**Date:** 2026-10-13
**Status:** Decided (exact cutoff to be confirmed)
**Options considered:** include everything, or exclude "In progress" complaints and recent data
**Decision:** Leave out "In progress" complaints (654,590). Leave out the latest partial month (October 2026 has only 10 days of data, 132,701 complaints) from trend charts and rates.
**Why:** Unfinished complaints have no outcome yet. The CFPB also says recent data is incomplete because companies have 15 days to respond. In the monthly chart, the partial month looks like a false drop.
**Trade-off:** The latest weeks are missing from charts. Draft cutoff: end of September 2026, to be revisited at the final run.

---

## 3. Standardize product names in the silver layer
**Date:** 2026-10-13
**Status:** To confirm
**Options considered:** keep original names, or map renamed products to one standard name
**Decision:** Map renamed products to one standard name (for example, the three "Credit reporting" variants) and keep the original column too.
**Why:** The same product appears under several names over time.
**Trade-off:** A wrong mapping would mix different products. I will confirm the renames in the CFPB field reference before merging.

---

## 4. Analyze credit reporting separately
**Date:** 2026-10-13
**Status:** Decided
**Options considered:** one analysis of all products, or credit reporting and other products separately
**Decision:** Analyze credit reporting on its own, and the remaining products together.
**Why:** The three credit reporting product names total about 15.0M of 18.3M complaints (about 82%), so an "all products" view would mostly describe credit reporting.
**Trade-off:** Two views to build and explain, but each is clearer.

---

## 5. Develop on a 1% random sample, with a fixed seed and the original 15 columns
**Date:** 2026-10-13
**Status:** Decided
**Options considered:** full 18.3M rows, a date-range slice, or a random sample
**Decision:** Take a 1% random sample of complaints from 2023 onward (151,161 rows) using `random_state=42`, keeping only the original 15 columns. Run the final pipeline on the full data.
**Why:** The full file is too large for my computer and for the usage limits of Databricks Free Edition. A date-range slice from 2023 still left 15.1M rows, so it was not small enough. The seed makes the sample reproducible, and keeping only the original columns keeps the bronze layer raw.
**Trade-off:** A sample can miss rare events, such as late responses (about 0.6% of complaints) or small companies. Final results will be checked on the full data.

---

## 6. Complaint ID is the key; the grain is one row per complaint
**Date:** 2026-10-13
**Status:** Decided
**Options considered:** use Complaint ID, or build a key from several columns
**Decision:** Use Complaint ID as the key of the fact table.
**Why:** The uniqueness check found 0 duplicate complaint IDs in 18.3M rows.
**Trade-off:** If a future data refresh contains duplicates, I need to re-run the check. It will be added to the data quality tests.

---

## 7. Flag date anomalies instead of deleting them
**Date:** 2026-10-13
**Status:** Decided
**Options considered:** delete the rows, correct the dates, or keep and flag them
**Decision:** Keep the rows and add a flag column (for example, `is_date_anomaly`). Exclude flagged rows only from days-to-company calculations.
**Why:** 7,050 complaints (about 0.04%), all from 2011 to 2014, have a sent date exactly 1 day before the received date. They do not affect analysis from 2015 onward, and deleting them would change complaint counts.
**Trade-off:** I do not know the cause, so I am not correcting the dates.

---

## 8. Describe the credit reporting surge without explaining its cause
**Date:** 2026-10-13
**Status:** Decided
**Options considered:** propose a cause, or describe the pattern only
**Decision:** Report what the data shows and make no causal claim.
**Why:** Monthly credit reporting complaints grew from about 181,000 (May 2024) to about 639,000 (September 2026), with a visible step from 267,923 (December 2024) to 365,867 (January 2025), about 37%. The data has no field that explains why.
**Trade-off:** The story is less dramatic, but it is defensible in an interview.

---

## 9. Submission channel does not separate credit reporting from other products
**Date:** 2026-10-13
**Status:** Decided
**Options considered:** use channel as an explanation of the surge, or rule it out
**Decision:** Do not use submission channel as an explanation.
**Why:** Since 2022, about 99.6% of credit reporting complaints and 93.6% of other complaints came through the web.
**Trade-off:** This answers part of business question 6 and leaves other patterns (company, state) to check.

## 10. Bronze keeps raw values, with renamed columns and two bookkeeping columns
**Date:** 2026-10-15
**Status:** Decided
**Options considered:** infer column types, or read everything as text
Decision: read all columns as text, rename columns to snake_case, and add _source_file and _loaded_at
**Why:** type guessing can silently change values, and Delta tables don’t accept spaces in column names by default
**Trade-off:** types must be converted in silver
**Reconciliation passed:** 151,161 rows, and null counts per column match the source file exactly.
