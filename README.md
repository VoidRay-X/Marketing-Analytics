# Marketing-Analytics

# Hyper-Personalized Customer Engagement Solution
> **End-to-End Analytics Framework for Data-Driven Customer Retention & Recommendation Systems**

## 1. Executive Summary & Business Context
In modern entertainment e-commerce and media ecosystems, generic bulk-marketing approaches yield diminishing returns. To maximize Customer Lifetime Value (CLV) and minimize churn, digital platform operations must transition toward **hyper-personalized communication strategies**.

This repository showcases an enterprise-ready analytics framework designed to power a **First-Ever Personalized Email Marketing Campaign** for an international media rental platform. By mining complex operational relational databases (encompassing historical transaction logs, customer inventory interactions, genre taxonomies, and talent metadata), this solution constructs a scalable data pipeline that engineers individualized customer behavior profiles. These profiles inject data-driven narrative hooks and context-aware media recommendations into the outgoing marketing framework, transforming cold historical data into functional growth assets.

### Core Problem Statement
The Marketing and Customer Analytics departments require the systematic synthesis of complex behavioral data points across the entire active customer database. The objective is to calculate each customer's primary genre affinities, benchmark their consumption metrics against peer cohorts, and generate highly specialized, un-watched recommendations (across categories and talent) while robustly flagging edge cases to prevent broken UI elements or friction in final delivery.

---

## 2. Targeted Personalization Data Matrix
To feed the creative copy layout designed by the marketing team, the analytics engine handles the extraction, transformation, and structural calculation of the following customer-specific data nodes:

| Data Points | Campaign Email Module | Business Requirement | Analytical Engine Implementation Logic |
| :---: | :--- | :--- | :--- |
| **1 & 4** | **Top 2 Categories** | Identify the top two content genres for each user based on lifetime rental velocity. | Aggregates views per customer across category dimensions; applies `DENSE_RANK()` windowing partitioned by customer descending. |
| **2** | **Primary Category Insights** | Calculate consumption depth within favorite genre: total watched, cohort average comparison, and percentile. | Computes raw count, derives global category averages via analytical windows, and utilizes `PERCENT_RANK()` across the database. |
| **5** | **Secondary Category Insights** | Quantify relative engagement within the runner-up favorite genre. | Extracts secondary category totals and divides them by absolute lifetime customer views to establish a precise percentage share. |
| **3 & 6** | **Category Film Recommendations** | Extract 3 highly popular, trending titles within the Top 2 genres that the user has **never** rented. | Excludes customer watch lists from available inventory; ranks residual titles via global rental counts to select the top 3. |
| **7, 8 & 9**| **Favorite Actor Feature** | Identify the customer's favorite actor and recommend 3 top-rated, un-watched films featuring them. | Counts talent occurrences within customer transaction tables; pulls the highest-scoring actor's filmography and filters out watched IDs. |

---

## 3. Architecture & Enterprise Technology Stack
To execute this with corporate-grade scale and reproducible isolation, a decoupled Modern Data Stack (MDS) strategy was defined:
* **Relational Core Database Engine (Microsoft SQL Server / SSMS):** Selected as the data source and heavy calculation environment. Managed via SQL Server Management Studio (SSMS), the platform utilizes T-SQL relational algebra to allow optimized execution of complex relational joins, analytical subqueries, and advanced windowing functions (e.g., DENSE_RANK(), PERCENT_RANK()).
* **Data Processing & Validation Layer (SQL Server / SSMS):** Managed entirely within SQL Server Management Studio using native database stored procedures and scheduled jobs. This layer orchestrates data pipelines, programmatically executes data quality metrics and validation constraints, and automates the export of final validated datasets directly at the database level.
* **Business Intelligence Framework (Tableau / Power BI):** Deployed to grant key project stakeholders total operational transparency over pipeline health and catalog coverage prior to campaign deployment.

---

## 4. Comprehensive Data Quality & Validation Protocol
Prior to triggering the recommendations matrix, an exhaustive data audit protocol is enforced across the relational schema to ensure 100% data integrity and prevent broken variables:

### A. Structural & Referential Integrity Checks
* **Entity Resolution Quality:** Run explicit checks ensuring zero orphaned keys exist between critical transactional layers (e.g., matching `rental.inventory_id` flawlessly to `inventory.inventory_id`).
* **Primary Key Uniqueness Constraint:** Confirm identifier configurations across entity tables (`customer_id`, `film_id`) contain zero duplicate records or physical collisions.
* **Null Value Scan:** Audit key operational metrics fields (`rental_date`, `return_date`, `customer_id`) to flag missing inputs and prevent empty data frames.

### B. Behavioral Log & Sanity Checks
* **Temporal Trajectory Validation:** Programmatically confirm that all transactional records obey logical linear constraints: **Rental Date <= Return Date**.
* **Outlier & Out-of-Bounds Audits:** Isolate customers with anomalous consumption velocities (e.g., standard deviations above normal thresholds like >10 movies simultaneously held) to filter out commercial accounts from standard customer profiles.

---

## 5. Pipeline ETL Processing Methodology
The data pipeline structures raw, highly normalized relational entities into flattened analytic states through five distinct calculation phases:

[Ingestion & Multi-Table Joins] 
       │
       ▼
[Aggregation Vector Engine]  ──► (Calculates customer-category & customer-actor counts)
       │
       ▼
[Window Optimization Engine] ──► (Computes cohort statistics & percentiles simultaneously)
       │
       ▼
[Antijoin Exclusion Filters] ──► (Removes specific customer watch histories)
       │
       ▼
[Recommendation Array Generation] ──► (Selects top 3 trending titles for final production)


---

## 6. Exception Handling & Business Logic Constraints
To ensure a seamless user experience, the analytics framework handles data limitations using explicit fallback logic, generating exception flags that tell the marketing platform how to adapt the email layout:

| Pipeline Trigger Event | Generated Integrity Flag | Automated UI / Marketing Adaptation |
| :--- | :--- | :--- |
| **Ultra-New / Lower Activity User**<br>(Insufficient historical data footprint) | `FLAG_ZERO_HISTORY_EXCLUSION` | Suppress the personalized email entirely; route the customer to a curated "Trending Platform Favorites" template. |
| **Skewed Historical Affinity**<br>(Customer has only rented from 1 category lifecycle) | `FLAG_SINGLE_CATEGORY_FALLBACK` | Dynamically restructure layout: replace the secondary category section with a universal top-rated platform recommendation module. |
| **Saturated Category Footprint**<br>(Customer has watched all available movies in their top genres) | `FLAG_INSUFFICIENT_REC_COUNT` | Backfill empty recommendation slots using collaborative filtering recommendations from adjacent, unexplored categories. |
| **Actor Recommendation Deficiency**<br>(Fewer than 3 un-watched films exist for their favorite actor) | `FLAG_ACTOR_REC_SHORTFALL` | Dynamically pull high-performing platform releases sharing the same director or thematic keywords. |

---

## 7. Executive Business Intelligence Dashboard Architecture
To give stakeholders full operational visibility before launching the campaign, a specialized, dual-layered BI layout was designed to verify campaign readiness.

### Dashboard Core KPIs
* **Audience Readiness Volume:** Count of clean, fully enriched customer records ready for immediate delivery.
* **Pipeline Exception Failure Rate:** Running breakdown tracking the distribution of safety fallback triggers.
* **Inventory Utilization Index:** Rate indicating how evenly the recommendation engine draws from the active content catalog.

### Structural Interface Layout
* **Tab 1: Campaign Validation & Data Integrity Ledger**
  * Contains heavy metrics cards reflecting data readiness across geographic regions.
  * Displays horizontal bar tracking of generated Exception Flags to let analysts quickly identify configuration bottlenecks.
* **Tab 2: Customer Affinity Profiles & Catalog Distribution Analytics**
  * Displays an interactive tree-map of customer distributions grouped by their #1 primary category.
  * Generates scatter plot distributions comparing individual consumption volume against average rental periods to reveal user engagement cohorts.

### Global Slicers & Filters
* `Customer Membership Status Tier` (e.g., Basic, Premium, Corporate)
* `Geographic Demographics / Store Location ID`
* `Data Integrity State` (Fully Validated vs. Flagged for Exception Restructuring)
