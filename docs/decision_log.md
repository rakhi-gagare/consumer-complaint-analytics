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

## 6. Which observable patterns accompany the credit reporting surge (submission channel, company, state)? This question is descriptive only. The project makes no claim about the cause.
**-** About 99.6% of credit reporting complaints since 2022 were filed through the web, compared with 93.6% for other products, so submission channel does not distinguish them.

**1.** Complaint ID is the key. No duplicates were found, so the fact table grain is one row per complaint.

**2.** Date anomalies are flagged, not deleted. 7,050 complaints (about 0.04%) have a sent date earlier than the received date. They stay in the data and are excluded only from days-to-company calculations.

**3.** The latest partial month is excluded from trend charts and rates.

**4.** Credit reporting volume grew steadily and accelerated from 2024, with a visible step from December 2024 to January 2025. This is described, not explained.

**5.** Submission channel does not separate credit reporting from other products, because about 99.6% of credit reporting complaints and 93.6% of other complaints came through the web.

### 7,050 complaints (about 0.04%), all from 2011 to 2014, have a sent date exactly 1 day before the received date. They are kept and flagged, and excluded from days-to-company calculations. They don’t affect analysis from 2015 onward.
