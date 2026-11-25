## 📌 Project Overview & Analytical Approach

Analyzed multi-year South Australia crash and casualty data in Python using statistical and visual workflows (pandas, numpy, scipy, matplotlib, seaborn). By separating incident frequency from casualty severity across time, location, road conditions, and driver behavior, the project isolates factors driving severe harm rather than just high-volume hotspots.

---

## 🔄 Data Workflow & Analytical Steps

### 1. Data Ingestion, Audit & Quality Profiling
- Loaded raw multi-year crash datasets and audited schema data types, null/missingness patterns, duplicate records, and distinct categorical distributions across spatial, temporal, infrastructural, and behavioral fields.
- Verified primary crash identifiers (`REPORT_ID`) to prevent double-counting when joining or aggregating multi-party vehicle and casualty records.
- Separated the full crash dataset from a dedicated **casualty subset** so injury severity and casualty dynamics could be evaluated accurately without diluting non-casualty property-damage records.

### 2. Data Cleaning & Domain Feature Engineering (Categorization)
- **Time & Calendar Engineering:** Extracted year, month, weekday/weekend flags, peak-hour bands, and **SA Public Holidays** from raw timestamps to capture behavioral time trends.
- **Regional Normalization:** Standardized suburb and regional labels (**Greater Adelaide vs. Regional SA**) for clean urban-versus-rural risk comparison.
- **Infrastructure & Speed Grouping:** Consolidated raw traffic attributes into clean analytical tiers:
  - **Road Group:** Grouped granular road layouts (e.g., intersections, roundabouts, divided/undivided roads) into standardized topology tiers.
  - **Speed Group:** Binned posted limits into operational speed zones (`<=50`, `60–70`, `80–90`, and `100+ km/h`).
  - **Control & Environment:** Standardized traffic controls (`Controlled` vs. `Uncontrolled`) and lighting/weather states.
- **Impairment Data Cleaning:** Flagged missing `DUI` and `Drugs` records as explicit `Unknown` rather than `0` to prevent metric distortion during slicing.

### 3. Normalized Rate & Severity Ratio Metrics
- Built reusable aggregation functions to compute **daily average rates** (`Crashes per Active Day` and `Casualties per Active Day`) normalized by active observation days rather than relying solely on raw totals.
- Introduced a **Casualty Ratio** metric (`Total Casualties / Total Crashes`) and **Severe/Fatal Share** to quantify how severe crashes are relative to incident frequency across different operating contexts.

### 4. Multi-Dimensional EDA & Scenario
