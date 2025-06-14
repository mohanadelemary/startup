# GetYourGuide Data Analyst Onboarding – Knowledge Base Skeleton

This file serves as a central place to plug in your growing knowledge base during onboarding. It mirrors the structure of your role and learning needs across data, experimentation, and automation at GYG.

---

## 📁 1. Data Landscape & Tables

### 1.1 Google Ads (SEM) – Core Tables
- `sem_reports_etl_google_ads.campaign_structure_enhanced`
  - ❑ Notes on schema (fields, nesting, partitioning)
  - ❑ Campaign types, labels, and how to filter
  - ❑ How to join with performance table
- `sem_reports_etl_google_ads.campaign_performance_enhanced`
  - ❑ Granularity (date, device, etc.)
  - ❑ Metrics available (clicks, cost_micros, conversions)
  - ❑ Conversion types breakdown

### 1.2 Search Terms & Keywords
- `sem_reports_google_ads.daily_search_term_view_performance`
- `sem_reports_etl_google_ads.dynamic_search_ads_search_term_view`
  - ❑ Use in analyzing DSA vs regular campaigns

### 1.3 Auction Insights
- ❑ Table name(s)
- ❑ Metrics (overlap rate, impression share, etc.)
- ❑ Best practices for interpretation

### 1.4 Attribution (Markov Chains)
- ❑ Table(s) used
- ❑ Difference between global vs path-level weights
- ❑ How adjusted conversions are computed

### 1.5 SEO Tables
- ❑ Main tables used for SEO traffic/conversion analysis
- ❑ How organic performance is compared to paid

### 1.6 Other Marketing Channels
- ❑ Overview of available tables (e.g., Meta, Display)

### 1.7 Shared Dimensions
- ❑ Accounts, campaigns, device types, regions
- ❑ Join keys and common pitfalls

---

## 📈 2. Experimentation

### 2.1 Types of Experiments
- A/B tests
- Geo holdouts
- Synthetic control (e.g., CausalImpact)
- MMM scenarios

### 2.2 Design Considerations
- ❑ Power, MDE, duration trade-offs
- ❑ Why GYG often prefers MMM over A/B
- ❑ When A/B is still appropriate

### 2.3 Implementation
- ❑ Where test/control assignments are stored
- ❑ Candidate selection
- ❑ Guardrails (metrics, traffic, eligibility)

### 2.4 Analysis Frameworks
- ❑ Diff-in-Diff
- ❑ Google’s CausalImpact
- ❑ CUPED / Regression adjustment
- ❑ Interpreting results: statistical vs practical significance

### 2.5 Reporting & Tools
- Looker dashboards (if any)
- Databricks notebooks
- Power calculation templates

---

## 🤖 3. Bidding, ROAS & Steering

### 3.1 ROAS Models & Elasticity
- ❑ What is marginal ROAS?
- ❑ Portfolio-level vs campaign-level granularity
- ❑ Interpreting lift and diminishing returns

### 3.2 Bidding Strategies
- Max Conv. Value vs tROAS
- Bid portfolio structure
- Migration rules (from broad to tighter control)

### 3.3 Steering Use Cases
- ❑ How ROAS elasticity is used to steer budgets
- ❑ Campaign grouping heuristics
- ❑ Role of seasonality & portfolio rebalancing

### 3.4 Model Inputs & Outputs
- ❑ Data sources used (what tables)
- ❑ Model outputs: curve, bid recommendation, risk estimates

---

## 🔧 4. Automation & Tooling

### 4.1 Databricks
- Notebooks used by your team
- Access controls
- Job orchestration & scheduling

### 4.2 Looker
- Explorers relevant to your work
- Custom fields & table calcs
- Filters, pivoting, download formats

### 4.3 AI Assistant Tools
- Usage of Cursor, GitHub Copilot, etc.
- Smart workflows for querying & documentation

### 4.4 GitHub
- Where notebooks and code are stored
- Guidelines for pushing updates and reviews

---

## 🧠 5. Strategic Thinking & Causal ML (Later Stage)

### 5.1 Uplift Modeling
- When to use uplift (post-AB)
- Data requirements and pipelines
- Model evaluation (Qini, uplift curves)

### 5.2 Causal DML / EconML
- When it's better than naive regression
- Connection to experimentation
- Libraries (EconML, DoWhy, CausalML)

### 5.3 MMM & Long-Term Impact
- Portfolio-level MMM for Paid Search
- Difference from short-term conversion lift
- Using MMM to inform budgets and tROAS

---

## 📚 6. Appendices & Links

- ❑ [Campaign Analysis Notebook – Databricks](https://dbc-d10db17d-b6c4.cloud.databricks.com/editor/notebooks/3531795185158304?o=4592942032988138)
- ❑ [Search Term Tables Documentation]
- ❑ [Markov Chain Attribution – Internal Docs]
- ❑ [Experimentation Decks – Google Slides]
- ❑ Add any other internal doc links here

---

This is a living doc – plug in learnings, cleaned SQL snippets, screenshots, and tips as you go. We’ll iterate and formalize it over time.
