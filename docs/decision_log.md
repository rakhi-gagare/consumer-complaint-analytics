# Decision Log

Each entry: what I decided, what else I considered, why, and the trade-off.

## 1. Main metric is relief rate, not timely response rate
**Date:** 2026-10-13
**Options considered:** timely response rate, relief rate, complaint volume
**Decision:** Use the share of complaints closed with relief as the main outcome metric.
**Why:** 18,218,355 of 18,329,250 complaints (99.4%) were answered on time, so the timely response rate cannot separate good companies from bad ones.
**Trade-off:** "Non-monetary relief" is a broad category, so the metric depends on how companies label their responses.

## 2. Exclude unfinished complaints and the most recent data from rates
**Date:** 2026-10-13
**Options considered:** include everything, or exclude "In progress" and recent complaints
**Decision:** Leave out "In progress" complaints (654,590) and the most recent weeks when calculating rates.
**Why:** Unfinished complaints have no outcome yet. The CFPB also says recent data is incomplete because companies have 15 days to respond.
**Trade-off:** The latest weeks are missing from trend charts. Exact cutoff date to be decided later.

## 3. Standardize product names in the silver layer
**Date:** 2026-10-13
**Options considered:** keep original names, or map renamed products to one standard name
**Decision:** Map renamed products to one standard name (for example, the three "Credit reporting" variants), and keep the original column too.
**Why:** The same product appears under several names over time.
**Trade-off:** A wrong mapping would mix different products. I will confirm the renames in the CFPB field reference before merging. (Status: to confirm.)

## 4. Analyze credit reporting separately
**Date:** 2026-10-13
**Options considered:** one analysis of all products, or credit reporting and other products separately
**Decision:** Analyze credit reporting on its own, and the remaining products together.
**Why:** The three credit reporting product names total about 15.0M of 18.3M complaints (about 82%), so an "all products" view would mostly describe credit reporting.
**Trade-off:** Two views to build and explain, but each is clearer.
