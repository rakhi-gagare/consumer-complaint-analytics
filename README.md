# Consumer Finance Complaint Resolution Analytics

End-to-end analytics project on the US CFPB Consumer Complaint Database: data pipeline, star schema, Power BI report, statistical analysis, and forecasting.

**Status:** In progress. Business problem and data exploration are complete. The data pipeline is being built.

---

## Business Problem

### Background
The US Consumer Financial Protection Bureau (CFPB) publishes complaints that consumers file about financial products and services. The public database holds about 18.3 million complaints from December 2011 to October 2026.

Complaint volume has grown sharply, from about 0.8 million in 2022 to about 5.4 million in 2025, with 2026 already above 5.6 million by early October. Credit reporting makes up about 82% of all complaints, and about 91% of 2026 complaints so far. Three companies (TransUnion, Equifax, and Experian) account for about 94% of credit reporting complaints.

### Problem Statement
Complaint volume is rising fast and is concentrated in one product area, so a compliance or customer operations team can't tell where to focus. About 99.4% of complaints are answered on time, so response speed alone doesn't show where service is weak. The team needs a clearer picture of where complaints are growing and which products and companies resolve them least often with relief to the consumer.

### Stakeholders (role-based, for the report)
- **Head of Compliance and Consumer Risk:** needs to know where complaint risk is growing.
- **Customer Operations Manager:** needs to know where complaints are resolved poorly and where volume will strain the team.

### Business Questions
1. How has complaint volume changed over time, and how much of the growth comes from credit reporting versus other products?
2. Which products and companies have the lowest rate of complaints closed with relief for the consumer?
3. How concentrated are complaints among a few companies, and has that changed over time?
4. When responses are late (about 0.6% of complaints), are they concentrated in particular companies, products, or periods?
5. What monthly complaint volume should we expect next quarter?
6. Which observable patterns accompany the credit reporting surge (submission channel, company, state)? This question is descriptive only. The project makes no claim about the cause.

---

## Metric Definitions (draft)

| Metric | Definition |
|---|---|
| Complaint volume | Count of complaints by month, product, and company |
| Relief rate | Complaints closed with monetary or non-monetary relief, divided by all closed complaints |
| Late response rate | Share of complaints where "Timely response?" is No |
| Days to company | Date sent to company minus date received |
| Concentration | Share of complaints held by the top 3 companies in a product |

**Rules**
- "In progress" complaints are excluded from outcome rates, because they are not finished.
- The most recent weeks of data are excluded, because companies have 15 days to respond.
- Rates are compared within a product, not across all products.
- Unclear closure values ("Closed", "Closed with relief") will be checked against the CFPB field reference before use.

---

## Data

- **Source:** [CFPB Consumer Complaint Database](https://www.consumerfinance.gov/data-research/consumer-complaints/) (public, updated daily)
- **Field reference:** https://cfpb.github.io/api/ccdb/fields.html
- **Size explored:** 18,329,250 rows and 15 columns
- **Date range:** December 2011 to 10 October 2026
- **Development sample:** 1% random sample of complaints from 2023 onward (about 151,000 rows)
- **Final run:** full dataset, to show the pipeline works at scale

The raw data is not stored in this repository. Download it from the source above.

### Data issues found so far
- The same product appears under several names (for example, three "credit reporting" labels and two "credit card" labels), which need standardizing.
- The Tags column is about 95% empty, and Company public response is about 45% empty.
- 2026 is a partial year, so comparisons with full years are labeled.

---

## Planned Approach

| Stage | What | Tools |
|---|---|---|
| 1. Explore | Profile the data and define metrics | Python (pandas) |
| 2. Bronze layer | Store raw data unchanged | Databricks, Delta tables |
| 3. Silver layer | Clean, standardize, and log removed rows | SQL, PySpark |
| 4. Gold layer | Star schema for reporting | SQL, Delta tables |
| 5. Data quality | Automated checks on each layer | SQL, Python |
| 6. Reporting | Dashboard with time intelligence and Row-Level Security | Power BI, DAX |
| 7. Analysis | Hypothesis tests and volume forecast | Python (statsmodels, scipy) |
| 8. Documentation | Architecture diagram, decision log, recommendations | GitHub |

---

## Limitations

- Complaint volume isn't adjusted for company size or customer base, so counts aren't a measure of quality or harm. The CFPB itself notes that low complaint volume does not mean low consumer harm.
- The data covers complaints that consumers chose to file, not all consumer experiences.
- The growth in complaints cannot be explained from this data alone.

---

## Repository Structure

```
├── README.md
├── docs/
│   └── decision_log.md
├── notebooks/
├── sql/
└── data/            (not committed)
```

## Decision Log

Key design decisions and the reasoning behind them are recorded in [docs/decision_log.md](docs/decision_log.md).

## Author

**Rakhi Gagare**, Data Analyst
https://www.linkedin.com/in/rakhi-gagare-045094234/?isSelfProfile=true | rakhigagare105@gmail.com
