# Startup: Overview

This document provides succinct coverage of key topics that need to be mastered during the onboarding process for the Senior Analytics Growth Marketing Manager role on the Paid Search team.

## Topics to Master

### [1. Google Ads Account Structure](#section-1-google-ads-structure--architecture)
- **a.** Account Architecture  
- **b.** Setup Process (CMS, Structure, Keywords, Customer Interest Translations and Aliases)  
- **c.** System Management (Operations, Changes, Shutdowns of Keywords and Other Components)


### [2.System Optimization Data Products](#section-2-system-optimization-data-products)
- **a.** RSA Optimization
- **b.** CIT Mining
- **c.** Bid Elasticity Mapping



### [3. Markov Chains Attribution Model](#section-3-markov-chains-attribution-model)


### 4. ROAS Goals and Tactics

### 5. Experimentation
- **a.** MIM (Marketing Incrementality Measurement)
- **b.** D&E (Design & Experimentation)
- **c.** Issues with Geo Studies vs. A/B Testing (Power, MDE)
- **d.** Tools, Apps, Notebooks, Databases
- Stefano sheet with log and different types of signals
- MRX insights presentations
- types of signals

### 6. DataBricks & Learning Our Data

### 7. Chiara's Workflow and Steering

---
DATA:

1. sem-reports-etl-google-ads in Databricks
2. Marketing Attribution Explore plus whatever Paul used for main performance dashboard
Both based on Markov Attribution Numbers
https://getyourguide.looker.com/dashboards/10618?Super+Account=&Account=&Campaign+ID=&Ca[…]ame=&Location+ID=&Report+Week+of+Year=&Report+Date=30+day








---


# Section 1: Google Ads Structure & Architecture

This section documents the core structure of Google Ads accounts used at GetYourGuide (GYG), with explanations on hierarchy, segmentation logic, keyword setup, and operational tooling. This is a live knowledge base to accelerate onboarding and deepen system understanding.

---

## 1.1 Overview of Structure

At GYG, the Paid Search structure follows a strict hierarchy:

- **MCCs / Superaccounts**  
  - One per language (e.g., English, German, French, Italian, Spanish)  
  - Each MCC is global and contains multiple accounts segmented by keyword type

- **Accounts within MCCs**
  - **Core Account**: General keyword-based campaigns (e.g., "paris tours")
  - **LOC Account**: Location-only keywords (e.g., "berlin")
  - **SEED Account (Near Me)**: "Things to do near me" type queries
  - **Brand Account**: Keywords that include "GetYourGuide" or variants
  - **Test**
  - SEED Contains CIT and CIT + near me keywords only & is controlled by location targeting

---

## 1.2 Campaign Level

- One campaign per **location** (city, country, area, or POI)
- Naming and structure follow specific conventions (see Cheat Sheet)
- Funnels define the type of query targeted

Examples:
- `Las Vegas` → Campaign with multiple ad groups for helicopter tours, museums, etc.
- `Eiffel Tower` → POI campaign with ad groups for tickets, guided tours, etc.

---

## 1.3 Ad Group Level

- Each campaign has multiple **Ad Groups**, structured by:
  - **Generic (TTD)**: e.g., "Things to do in Las Vegas"
  - **Category-specific (non-TTD)**: e.g., "Las Vegas Helicopter Tours"
  - **Location-to-location (f7)**: e.g., "Day trips from London to Stonehenge"
  - **Dynamic Search Ad Group (DSA)**: Targets entire website content

- The structure varies depending on **location type**:
  - For **country, area, city, neighborhood**:
    - TTD (generic) ad group
    - CI/CIT-specific (f4/f8) ad groups
    - Location-to-location (f7) ad group
    - DSA ad group
  - For **POI**:
    - Generic ad group
    - CI/CIT-specific (f5) ad group
    - DSA ad group

---

## 1.4 Keyword Generation Logic

- Keywords are generated from:
  - CMS Locations (incl. aliases like "Eiffel Tower", "Tour Eiffel", etc.)
  - Customer Interests (e.g., Tickets, Tours, Cruises)
  - CIT Translations in 18+ languages

- Keyword Templates define ordering:
  - `{{ci_translation}} {{location_alias}}`
  - `{{location_alias}} {{ci_translation}}`
  - Some are prefixed/suffixed depending on rules
  - All KW's are setup per AG in both Exact and Broad Match

---

## 1.5 Supporting Automation & Operations

### Tools:
- **Mutation Notebook**: Used to apply changes (keywords, ad groups, etc.)(DB link in Resources)
  - `%run /Shared/Paid Search/utils/mutation`
- **SEM Operations Portal**:
  - SQR Moderation
  - Customer Interest Management
  - CIT Translations & Categorization
- **SEM Assets Portal**:
  - Headlines, descriptions, sitelinks
- **SEM Bidding Center**:
  - Assign ROAS targets
  - Campaign-level bidding strategies

---

## 1.6 Inventory & Performance Filters

Automated systems ensure quality of campaigns:

- **Inventory Watcher**:
  - Pauses ad groups if LP has fewer than X valid activities
  - Thresholds depend on landing page type (city, POI, country, etc.)
- **Autopause**:
  - Stops low-performing keywords
- **Demand Service**:
  - Checks search volumes for for the keywords list before they are created in the structure.
- **Budget Watcher**:
  - Watches budgets and provides alerts when limits are near.
- **Audience and Targeting Service**:
  - Defines language and location targeting setup.

---

## 1.8 Data Sources

- `sem_config.google_ads_account` – Definitions of MCCs, accounts, naming structure
- `sem_external.gyg_location` – Location metadata and types
- `catalog__location` – Canonical GYG locations used for segmentation
- `sem_config.customer_interest` – Definitions of interests and mappings
- `sem_config.customer_interest_translation` – CITs per language and alias
- `sem_config.keyword_templates` – Generation rules for keywords
- `sem_config.location_alias` – Alternate names and label options for locations
- `sem_config.landing_page_thresholds` – Activity requirements by LP type
- `%run /Shared/Paid Search/utils/mutation` – Mutation notebook used for manual overrides

---

## 1.9 Reference Links
- [Current Paid search account structure](https://docs.google.com/document/d/10nW54FJwjJne3-n_2afz3mp-ftzpk5sNq2FmTBTI3rM/edit?tab=t.0)
- [Paid Search DQ&A](https://docs.google.com/presentation/d/1SyPOPFvrvUw4so9gchpnQXx2nnkUerz88RUNMYJFFLA/edit#slide=id.gcefe29681b_0_1054)
- [Account Naming Convention Doc](https://docs.google.com/document/d/1YkA0sHTlBDBzGBLcydiAiNL0QzfFAfs-4EU6LlEwXWw/edit?tab=t.0#heading=h.rg1a4nx68vwx)
- [Paid Search Structure - Explanation & Templates](https://docs.google.com/document/d/10nW54FJwjJne3-n_2afz3mp-ftzpk5sNq2FmTBTI3rM/edit?tab=t.0#heading=h.3kv3bcy9mkz)
- [CI & CIT Mapping Logic](https://docs.google.com/document/d/1slscbMpMqc9Awyf97uj1okvpxY-YwdPbTgTkJpeDH14/edit?tab=t.0#heading=h.tsekf05qwcgy)
- [Master CI/CIT Table](https://docs.google.com/document/d/1slscbMpMqc9Awyf97uj1okvpxY-YwdPbTgTkJpeDH14/edit?tab=t.0)
- [G Ads / Product Scope](https://docs.google.com/presentation/d/1p8IQ6Xnq-F-88gd_A-lB2-BkLzYZelKDoHePFA9IPQ8/edit?slide=id.g713984b86b_0_0#slide=id.g713984b86b_0_0)
- [Mutation Notebook](https://dbc-d10db17d-b6c4.cloud.databricks.com/editor/notebooks/3194791989752051?o=4592942032988138#command/7347089574575382)
- [SEM Operations Portal](https://sem-admin.gygadmin.com/rsa-assets?page=1&limit=10&filters=%7B%22isSource%22%3A3%7D)
- [SEM Bidding Center](https://sem-bidding-center.gygservice.com/google/steering/all/)
---

# Section 2: System Optimization Data Products

## 🔧 a. RSA Optimization

### 🎯 Purpose
Optimize asset rotation in **Responsive Search Ads (RSAs)** by replacing underperforming assets and inserting new, high-performing ones using a **LightGBM LambdaRank model**. This enables continuous improvement of ad relevance and performance.

---

### 🧠 Overview of Solution
- Each RSA contains up to **15 headlines** and **4 descriptions**.
- Weekly optimization runs evaluate existing assets and recommend replacements.
- Two models (headlines & descriptions) are trained using **10 weeks of historical data**.
- The system predicts which new eligible assets are most likely to increase impression share.

---

### 🧪 Model & Approach
- **Model Type**: LambdaRank (LightGBM)
- **Target**: Impression share rank in the following week
- **Key Evaluation Metrics**: nDCG, MAP, Spearman & Pearson correlations

#### Ranking Performance
| Metric       | Headlines        | Descriptions     |
|--------------|------------------|------------------|
| nDCG         | @3/5/10: 0.936/0.932/0.938 | @1/2/4: 0.986/0.99/0.993 |
| MAP          | @3/5/10: 0.546/0.347/0.176 | @1/2/4: 1.0/0.51/0.42     |
| Spearman     | 0.78             | 0.91             |
| Pearson      | 0.63             | 0.73             |

---

### 🔁 Optimization Flow
1. Train model on past 10 weeks of performance.
2. Predict relevance scores for all eligible assets.
3. Filter assets by similarity:
   - **IOU** (n-gram overlap)
   - **Cosine similarity** (spacy embeddings)
4. Re-rank headlines using **Maximal Marginal Relevance (MMR)**.
5. Remove underperformers (e.g., <1.5% impression share for headlines).
6. Insert top-ranked eligible assets.

---

### 🧪 Related Concepts & Experiments
- **V3 Uplift Optimization**: Probabilistic asset selection based on uplift score.
- **Asset Incubation Period**: Time-based inclusion for new creative assets.
- **LLM Relevance Feedback**: Proposed enhancement using GPT-style models to score or rewrite ad assets.
- **Relevance Modeling**: Combines heuristics, text similarity, and performance metrics to flag poor fits.

---

### 📦 Inputs & Outputs

**Inputs**:
- `sem_reports_google_ads.daily_ad_structure`
- Asset metadata tables (`test.sl_ad_optimization_rsa_eligible_assets`, etc.)
- Ad group signals (location, funnel, customer interest)

**Outputs**:
- Asset rankings
- Asset inclusion/removal lists
- Dashboards (Datadog, Looker)
- MLflow tracking for headline/description model performance

---

### 📚 Resources
- [Project Overview: RSA ML Optimization](https://docs.google.com/document/d/1kgSZA_-6LOItHly44_Cf6I3V1tOgq_8UERSwKfqM3OM/edit)
- [Technical Implementation: RSA ML Optimization (V2)](https://docs.google.com/document/d/1Ii637rkQwAA-7x9Bejctq-FusFngz8sFpctig1uL-bQ/edit)
- [RSA Optimization V3 (Legacy)](https://docs.google.com/document/d/1Y4CbEM0R8vtUibbwdWUlQsaUKeaZoNN0lhlM0mmWaec/edit)
- [RSA Creative Relevance Feedback & Generation](https://docs.google.com/document/d/1baeZQmCnBGbzotxYCDYzNTAZBAyckhydsJjoINluGXA/edit)
- [Project Home: RSA Asset Relevance](https://docs.google.com/document/d/1Zlgfweqt5JaPj8dSoIPZVCMmlz0vVNXd7MGeXGb1vsU/edit)
- [RSA Optimization GitHub Repo](https://github.com/getyourguide/rsa-ad-optimization)
- MLflow Tracking: [Headlines Model](https://dbc-d10db17d-b6c4.cloud.databricks.com/ml/experiments/251692159741683?o=4592942032988138) · [Descriptions Model](https://dbc-d10db17d-b6c4.cloud.databricks.com/ml/experiments/4406261186561167?o=4592942032988138)

---

## 🔧 b. CIT Mining (Customer Interest Topics)

#### 🎯 Purpose
Mine and classify **Customer Interest Translations (CITs)** from Google Search Query Reports (SQRs) using a spaCy-based NER pipeline. The output enriches GYG's keyword targeting by uncovering long-tail interests and alternative phrasings across multiple languages.

---

### 🧠 Overview
- Uses a **spaCy v3 NER model** fine-tuned on internal keyword templates to extract **CITs and location aliases** from SQR data.
- A **CI classifier** maps new entities to the most likely canonical Customer Interest (CI).
- Outputs are filtered via **robust post-processing**, enriched with performance-based feedback loops and monitored for quality post-upload.

---

### 🔬 Technical Components

#### 📦 Training Data
- Derived from marketing keyword templates: e.g., `"canal tour in amsterdam"` = `CIT: canal tour`, `Location: amsterdam`
- Hardcoded tokens within keyword templates are annotated as CITs (excluding stopwords)
- Trained incrementally using new keyword data or fully re-annotated when needed

#### 🤖 NER Model
- Uses spaCy's large multilingual base models and transfer learning
- Language-specific config files define:
  - Loss functions
  - Batching strategy
  - Early stopping
- CITs can be incrementally retrained or reset from base as needed

#### 🔁 CI Classification Model
- Majority vote across 3 multilingual sentence encoders (e.g. multilingual-USE variants)
- If unsupported, CITs are translated to English and processed
- Helps smaller-language CITs benefit from richer related-language coverage

---

### 🧹 Post-processing Filters
Applies layered rejection rules before CITs are added to the database:

- ❌ Wrong language (detected via 3 voting models)
- ❌ Contains unwanted terms (e.g., “visa”, “voucher”, brand names)
- ❌ Low semantic relevance (measured via similarity to CI and other CITs)
- ❌ Location masquerading as CIT (e.g. “Spreefahrt”)
- ❌ Negative brand terms or incomplete phrases

Figma: [CIT Post-processing Diagram](https://www.figma.com/file/2DyRBj4BcIxF2FwvI5RqAD/SQR-CIT-post-processing?type=whiteboard&node-id=0-1)

---

### 🔄 Post-upload Clean-up
After CITs are added to ads:
- Evaluated using:
  - Quality score (avg or weighted by clicks)
  - Post-click quality score (Google Ads)
  - Predicted CTR
- CITs below thresholds are auto-flagged or paused, others are sent for manual review

---

### 👁️‍🗨️ Manual Review Loop
- Low-confidence CITs are sent to Paid Search for validation
- Manual labels (accepted/rejected + reason) are fed back into the model
- CITs are reviewed regularly based on:
  - Click volume
  - Demand potential
  - Confidence score
- Top-performing CITs with borderline flags may still be retained

---

### 📈 Monitoring & Quality Metrics

**Pipeline Health**
- # of CITs added per language / account / CI
- Keyword creation rate from CITs
- CIT rejection reason breakdown

**Model Metrics**
- F1 score, precision, recall
- Drift detection via Arize for:
  - CI classification model
  - Semantic similarity filters

**Business KPIs**
- Avg performance of keywords using new CITs
- Pre- vs post-cleanup keyword quality

---

### 📂 Tables, Code & Workflow
- GitHub Repo: [sqr-query-coverage](https://github.com/getyourguide/sqr-query-coverage)
- Pipeline: Airflow DAG (batch inference, retraining configurable)
- CIT review tables: stored in GDP and tracked by PS

---

### 📚 Resources
- [Project Home: Entity Mining (CITs)](https://docs.google.com/document/d/1FzoStuuAOv47gjrMoXG9JyPMAkOspJKFXiZDRAmzxyQ/edit)
- [Technical Implementation: CIT Mining](https://docs.google.com/document/d/13Ndw1CVG5mRgA6tCQmrglnPWVHAJgBBenWq2mk0mUVc/edit)
- [CIT Post-processing Figma](https://www.figma.com/file/2DyRBj4BcIxF2FwvI5RqAD/SQR-CIT-post-processing)
- [CIT Productionization Figma](https://www.figma.com/file/qiwDvbIxj1h41KveELVvit/SQR-Productionalization)
- [CIT Review Guidelines (internal)](https://docs.google.com/document/d/1TsBKw68aHqQlHxw-iuseJ-20q7v-AS_1G_3h0iFgFLA/edit)

---

## 🔧 c. Bid Elasticity Mapping (tROAS Recommender)

#### 🎯 Purpose
Recommend revenue-maximizing **target ROAS (tROAS)** values at the portfolio level by analyzing **Google Ads bid simulation data** and modeling **marginal return on ad spend (mROAS)**.

---

### 🧠 Core Idea
- Each **Google Ads portfolio** has a target ROAS that controls Smart Bidding.
- The **return from incremental spend varies** across portfolios.
- This system analyzes **Google’s 7-day bid simulation data**, fits elasticity curves, and optimizes spend distribution to **maximize total MC-attributed revenue** without reducing overall ROAS.

---

### 📊 Data Sources
- Bid simulation data: `sem_reports_google_ads.daily_bidding_strategy_simulation`
- Cost/revenue data (MC attribution): `fact_attribution`
- Maturation multipliers: stored in DBFS, used to estimate final MC revenue from clicks
- Snapshot tables: ensure **idempotent historical runs** by freezing inputs per date

---

### 🧮 Modeling

#### Revenue Curve Fitting
- Applies scaling factor to adjust Google's DDA revenue to GYG’s MC-attributed revenue
- Fits the curve:  
  `f(cost) = a * log(b * cost + 1) + c`
- Fitted via `scipy.curve_fit()` per portfolio
- mROAS (marginal ROAS) = slope at current cost point

#### Optimization Objective
- **Goal**: Reallocate budget to portfolios with higher marginal ROAS (mROAS)
- Method: `scipy.optimize.minimize()` using Sequential Least Squares Programming
- Constraints:
  - Keep current overall ROAS stable
  - Avoid underfunding portfolios (e.g., `cost_lower_bound`)
  - Respect tROAS granularity (based on Google’s sim points)

#### Output: tROAS Recommendations
Each portfolio receives:
- `rec_tROAS`: the new target ROAS recommendation
- `rec_tROAS_change`: change vs current tROAS
- `expected_cost`, `expected_revenue_mc`, and change factors

---

### 🛠️ Config Highlights
| Parameter                     | Purpose |
|------------------------------|---------|
| `REVENUE_TYPE`               | Use DDA (Google) or MC (GYG) revenue |
| `OPTIMIZATION_OBJECTIVE`     | Revenue or net profit |
| `ROAS_TOLERANCE`             | Allow small ROAS dip for higher revenue |
| `OVERALL_ROAS_OPTIMIZATION_TARGET` | Optional target override |
| `COST_LOWER_BOUND`           | Minimum spend floor per portfolio |
| `EXCLUDE_FROM_OPTIMIZATION`  | List of portfolios to skip |
| `SNAPSHOT_COST_NR_TABLE`     | Stores per-run input snapshot |

---

### 📈 Output Table Fields
| Field                       | Description |
|----------------------------|-------------|
| `rec_troas`                | New recommended target ROAS |
| `mroas_mc`                 | Marginal ROAS (GYG MC) |
| `expected_revenue_mc`      | Projected MC revenue post-change |
| `expected_roas_mc`         | Projected MC ROAS post-change |
| `rec_troas_change`         | Δ vs. current tROAS |
| `troas_history_value_days` | 10-day history of past tROAS values |
| `current_cost`             | Current spend for the portfolio |
| `report_date`              | Date of the recommender run |

---

### ⚙️ Execution & Monitoring

- Runs **daily** via Airflow DAG: `gdp_roas_elasticity` at **12:42 CET**
- Output tables:
  - `roas_elasticity.troas_steering_recommendations`
  - `roas_elasticity.troas_steering_recommendations_history`
- Testing:
  - CI/CD runs full recommender on merge to `main` or commits with `[BUILD]`
  - E2E tests assert output table is written to `roas_elasticity_testing`

---

### 💻 Manual Overrides
- A Databricks notebook allows:
  - Manual target setting
  - Scenario testing (different ROAS goals)
  - Visual UI to compare outcomes

---

### 📚 Resources
- [GitHub: gdp-ml-pipelines - roas-elasticity](https://github.com/getyourguide/gdp-ml-pipelines/blob/main/pipelines/roas-elasticity)
- Internal Datadog & Airflow monitoring dashboards (linked in repo)
- [ROAS Recommender Notebook (Manual Runs)](https://databricks.com/...) *(add link if available)*


---

# Section 3: Markov Chains Attribution Model


Rather than assigning global average weights to each channel, GYG's implementation assigns **credit to each channel within a user journey**, based on **how critical its presence is in the probability of conversion**.

> The ethos: **Move from fixed heuristics to learned, probabilistic, path-aware attribution that reflects real user behavior.**

---

## 🧠 Model Logic (Simplified)

### 🧩 Markov Chains
- A Markov Chain models **sequences of events (channel visits)** and the **probability of reaching a conversion**.
- States = acquisition channels; Terminal states = conversion or drop-off.
- The probability of a conversion path is calculated based on transition probabilities between channels.

### 🪄 Channel Attribution Logic
There are two main components:

#### 1. **Removal Effects (Global Concept)**
- Simulate removing a channel from all paths.
- Measure the drop in total conversion probability (e.g., if Paid Search is removed, does overall conversion drop by 15%?).
- This gives **global channel importance**.

#### 2. **Path-Level Attribution (Used at GYG)**
- For every *observed* path, calculate the path’s total probability of conversion.
- Distribute conversion value across the **channels in that path**, proportionally to their marginal contribution to that specific sequence.
- Revenue attribution is thus **fractionally assigned per path**.

Example:
```
Path: Display > Email > Paid Search > Conversion  
Revenue: €200  
Channel weights in this path: [0.1, 0.3, 0.6] → Attribution: [€20, €60, €120]
```

---

## 📈 Output and Use Cases

### ✅ Key Outputs
- **Revenue attributed per channel group or individual channel**
- **Campaign or portfolio level attribution** possible if channel-to-campaign mappings are known
- **Adjusted ROAS / tROAS** using path-based conversion value

### 💡 Used For:
- Smarter tROAS steering
- Campaign and budget optimization
- Understanding upper funnel and assist value
- Pre-testing incrementality estimation

---

## 🎯 How Markov Attribution Feeds Into Bidding Strategy at GYG


### 🔁 Attribution Feedback Loop (Simplified)
1. User Paths → Tracked & Modeled via Markov Chains  
2. → Attributed Conversions (per path, per channel)  
3. → Aggregated in dashboards (Looker)  
4. → Used to steer budget/tROAS inputs into Bidding Center  
5. → Strategy results evaluated again via Markov KPIs

### ⚠️ Clarification
Markov attribution is:  
- ❌ Not sent to Google Ads as real-time conversion actions.  
- ✅ Used outside Google to inform bidding strategies, targets, and budget allocation.

---

## 🔍 Key Advanced Concepts

### 📊 Random weights + optimization
- Initial channel weights in a path are **randomly assigned**.
- An optimization algorithm (e.g. gradient descent) iteratively adjusts them to **minimize loss**, defined as the **error between predicted and observed conversions**.
- This aligns with packages like [`ChannelAttribution`](https://cran.r-project.org/web/packages/ChannelAttribution/index.html) in R and [this Python port](https://github.com/DavideAltomare/ChannelAttribution/blob/master/python/README.md).

### 🧪 Testing & Validation
- While the model is unsupervised, GYG tested it by comparing to Position-Based models and running **incrementality tests** (e.g., regional holdouts or channel-specific pauses).
- Expected outcomes: model should reflect actual lift in holdout vs test regions.

---

## 📂 Internal Resources and Links

### 📚 GYG Blog Posts
- [Understanding Data-Driven Attribution Models](https://www.getyourguide.careers/posts/understanding-data-driven-attribution-models)
- [Deploying and Pressure Testing a Markov Chains Model](https://www.getyourguide.careers/posts/deploying-and-pressure-testing-a-markov-chains-model)

### 📦 Attribution Package
- [ChannelAttribution on CRAN (R)](https://cran.r-project.org/web/packages/ChannelAttribution/index.html)
- [ChannelAttribution Python GitHub](https://github.com/DavideAltomare/ChannelAttribution/blob/master/python/README.md)
- [ChannelAttribution PDF Docs (R)](https://cran.rstudio.com/web/packages/ChannelAttribution/ChannelAttribution.pdf)

### 🧭 BI & Internal Documentation
- [GYG Acquisition Channels & Channel Groups](https://stack.gygservice.com/docs/default/component/bi-dataguide/acquisition-channels/)


---

## ❓ Open Questions

1. **Granularity**: Does the final attribution ever happen on the *campaign* or *campaign type* level? If so, how is the mapping done?
2. **Revenue attribution math**: Are **all final revenue calculations based strictly on the sum of actualized fractional path-level weights** (not global removal effects)?
3. **Validation**: How exactly was the model tested against real-world outcomes? What does a holdout experiment or lift test look like in this context?
4. **Optimization Process**: In \"random weights + minimized loss\"—how exactly is that defined? How does this map to `ChannelAttribution`'s loss function and algorithmic backend?
5. **Access to raw data**: Where can we inspect raw paths, attribution weights per path, or simulated conversion drops?
6. **Codebase**: Where is the Markov pipeline implemented and maintained internally? Is it reproducible in Looker or Databricks?
7. **Usage**: Who currently uses the *path-level attribution* outputs? Is it feeding back into Google Ads bidding, portfolio steering, CRM planning, or budgeting?
8. **Data**: Best sources to use for everything.
9. Where can see overview of the MC model weights and values per different LOD's

---

## 📌 Additional Mentions

### 🔖 Landing Page Attribution Model
- Managed through the **Dollar Sign Tool** in GYGadmin.
- Used for: POI-level revenue analysis, supply-side reporting, and manual overrides of automated POI mappings.

### 💬 Post-Click Attribution?
- Is all attribution based on **post-click paths only**?
- If yes, what are the limitations for channels like **Display & Paid Social**, where much of the impact may be **view-through**?


