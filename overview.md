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

### 3. Markov Chains Attribution Model

### 4. ROAS Goals and Tactics

### 5. Experimentation
- **a.** MIM (Marketing Incrementality Measurement)
- **b.** D&E (Design & Experimentation)
- **c.** Issues with Geo Studies vs. A/B Testing (Power, MDE)
- **d.** Tools, Apps, Notebooks, Databases

### 6. DataBricks & Learning Our Data

### 7. Chiara's Workflow and Steering

---


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
