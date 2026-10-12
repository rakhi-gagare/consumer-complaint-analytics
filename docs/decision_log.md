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
**Why:** The full file is too large for my computer and for the usage limits of Databricks Free Edition. A date-range slice from 2023 still left 15.1M rows, so it was not small enough. The seed makes the sample reproducible, and keeping only the original columns keeps the bronze
