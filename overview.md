# Startup: Overview

This document provides succinct coverage of key topics that need to be mastered during the onboarding process for the Senior Analytics Growth Marketing Manager role on the Paid Search team.

## Topics to Master

### [1. Google Ads Account Structure](#section-1-google-ads-structure--architecture)
- **a.** Account Architecture  
- **b.** Setup Process (CMS, Structure, Keywords, Customer Interest Translations and Aliases)  
- **c.** System Management (Operations, Changes, Shutdowns of Keywords and Other Components)

### 2. System Optimization Data Products
- **a.** RSA Optimization
- **b.** Bid Elasticity Mapping
- **c.** CIT Mining

### [3. Markov Chains Attribution Model](#section-3-markov-chains-attribution-model)


### 4. ROAS Goals and Tactics

### 5. Experimentation
- **a.** MIM (Marketing Incrementality Measurement)
- **b.** D&E (Design & Experimentation)
- **c.** Issues with Geo Studies vs. A/B Testing (Power, MDE)
- **d.** Tools, Apps, Notebooks, Databases

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

---

## 1.5 Supporting Automation & Operations

### Tools:
- **Mutation Notebook**: Used to apply changes (keywords, ad groups, etc.)
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
  - Attaches volume or filters keywords pre-activation? (To be confirmed)
- **Audience, Targeting, Budget Services**:
  - Still under clarification – may dynamically adjust campaign inputs

---

## 1.7 Open Questions to Clarify

- What defines TTD vs non-TTD internally in GYG? answered
- what about seed, near me and other
- what about pmax and stuff
- what about actual super account structure now
- 
- Are generic ad groups always tagged as TTD?
- How are keywords moderated from SQR?
  - Manual only or performance-based inclusion?
- Are new LPs auto-generated when new CIs/locations emerge?
- Keyword match type setup: are both exact & broad always included?

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

✅ **Next:** Section 2 – System Optimization Data Products (RSA Optimization, Bid Elasticity, CIT Mining)




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

---

## 📌 Additional Mentions

### 🔖 Landing Page Attribution Model
- Managed through the **Dollar Sign Tool** in GYGadmin.
- Used for: POI-level revenue analysis, supply-side reporting, and manual overrides of automated POI mappings.

### 💬 Post-Click Attribution?
- Is all attribution based on **post-click paths only**?
- If yes, what are the limitations for channels like **Display & Paid Social**, where much of the impact may be **view-through**?


