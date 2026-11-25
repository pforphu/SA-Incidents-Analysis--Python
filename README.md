## 📌 Project Overview & Analytical Approach

Analyzed multi-year South Australia crash and casualty data in Python using statistical and visual workflows (pandas, numpy, scipy, matplotlib, seaborn). By separating incident frequency from casualty severity across time, location, road conditions, and driver behavior, the project isolates factors driving severe harm rather than just high-volume hotspots.

---

## 🔄 Data Workflow & Analytical Steps

---
### 1. Data Ingestion, Audit & Quality Profiling
- Loaded raw multi-year crash datasets and audited schema data types, null/missingness patterns, duplicate records, and distinct categorical distributions across spatial, temporal, infrastructural, and behavioral fields.
- Verified primary crash identifiers (`REPORT_ID`) to prevent double-counting when joining or aggregating multi-party vehicle and casualty records.
- Separated the full crash dataset from a dedicated **casualty subset** so injury severity and casualty dynamics could be evaluated accurately without diluting non-casualty property-damage records.

---
### 2. Data Cleaning & Domain Feature Engineering (Categorization)
- **Time & Calendar Engineering:** Extracted year, month, weekday/weekend flags, peak-hour bands, and **SA Public Holidays** from raw timestamps to capture behavioral time trends.
- **Regional Normalization:** Standardized suburb and regional labels (**Greater Adelaide vs. Regional SA**) for clean urban-versus-rural risk comparison.
- **Infrastructure & Speed Grouping:** Consolidated raw traffic attributes into clean analytical tiers:
  - **Road Group:** Grouped granular road layouts (e.g., intersections, roundabouts, divided/undivided roads) into standardized topology tiers.
  - **Speed Group:** Binned posted limits into operational speed zones (`<=50`, `60–70`, `80–90`, and `100+ km/h`).
  - **Control & Environment:** Standardized traffic controls (`Controlled` vs. `Uncontrolled`) and lighting/weather states.
- **Impairment Data Cleaning:** Flagged missing `DUI` and `Drugs` records as explicit `Unknown` rather than `0` to prevent metric distortion during slicing.

---
### 3. Normalized Rate & Severity Ratio Metrics
- Built reusable aggregation functions to compute **daily average rates** (`Crashes per Active Day` and `Casualties per Active Day`) normalized by active observation days rather than relying solely on raw totals.
- Introduced a **Casualty Ratio** metric (`Total Casualties / Total Crashes`) and **Severe/Fatal Share** to quantify how severe crashes are relative to incident frequency across different operating contexts.

---
### 4. Multi-Dimensional EDA & Scenario
- Objective: Analyze how combined factors impact crash severity across South Australia.
- Comparison: Contrast objective conditions (time, day type, holidays, road infrastructure) against behavioral factors (DUI Involved, Drugs Involved).
- Metric: Measure severity using Casualty Ratio ($\text{Total Casualties} / \text{Total Crashes} \times 100$).
- Visualization: Use side-by-side horizontal bar charts to highlight top high-risk scenarios for each group.

---
## Analysis Outcomes

### Daily Total Units vs. Casualties Distribution

<img src="Pics/Daily_Boxplot.png" width="550" alt="PL300" style="margin-left:-20px;">

* **Central Tendency & Spread:** Daily `Total Units` is centered around ~77 with a much wider spread (IQR ~61–92) than `Total Cas`, which sits around ~12 with a tighter range (IQR ~9–16).
* **Skewness & Outliers:** Both metrics show right-skewed behavior with notable high-count outlier days—up to ~163 for `Total Units` and ~37 for `Total Cas`—plus a low outlier near ~9 for `Total Units`.
* **Takeaway:** Daily vehicle/unit volume fluctuates heavily on peak days, while daily casualty counts stay comparatively low yet experience spike days worth inspecting further.

---
### Temporal Trends: Crashes & Casualties by Year and Month

<img src="Pics/TotalCrash_Year_Month.png" width="550" alt="PL300" style="margin-left:-20px;">

* **Yearly Trend (2021–2025):** Annual crashes dipped slightly in 2022 (~12,350) before rising steadily to a peak in 2025 (~13,900), while casualties stayed relatively stable around ~4,350–5,350 per year.
* **Monthly Pattern:** Crash volume peaks in **May** (~6,100) and **March** (~6,000), and hits its lowest point in **January** (~4,750).
* **Takeaway:** While overall crash frequency has climbed back up through 2025, casualty counts remain comparatively flat across both years and months.

---
### Temporal Patterns: Crashes & Casualties by Day of Week and Hour
<img src="Pics/Crash_Week_Hour.png" width="550" alt="PL300" style="margin-left:-20px;">

* **Day of Week:** Crashes rise steadily across weekdays to peak on **Friday** (~10,900 crashes, ~3,800 casualties), then drop sharply on weekends—lowest on **Sunday** (~6,400 crashes, ~2,700 casualties).
* **Hourly Bimodal Peaks:** Activity follows two commuter rush-hour surges—a morning peak at **8:00 AM** (~5,000 crashes) and a larger afternoon peak at **3:00 PM** (~6,100 crashes, ~2,250 casualties), with minimal volume overnight (3:00–4:00 AM).
* **Takeaway:** Crash and casualty volumes track commuter traffic tightly, though weekends show a slightly higher casualty-to-crash proportion despite lower overall incident counts.
