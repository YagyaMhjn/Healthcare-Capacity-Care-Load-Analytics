# Healthcare Capacity & Care Load Analytics

## Project Overview

This project analyzes daily operational data from the **Unaccompanied Alien Children (UAC) Program** to evaluate healthcare system capacity, care load, and the balance between inflows and outflows across the CBP and HHS care pipeline.

The analysis covers **1,075 calendar days from January 12, 2023 to December 21, 2025**. Operational data is transformed into healthcare capacity metrics, time-series indicators, backlog measures, volatility measures, and stress signals to identify periods of elevated and sustained system pressure.

> **Current status:** Exploratory Data Analysis (EDA), metric engineering, KPI development, and outcome analysis are completed. The Streamlit dashboard is planned as the next stage and has not yet been implemented.

---

## Problem Statement

The UAC Program operates as a dynamic care pipeline involving:

- Intake into CBP custody
- Transfer to HHS care facilities
- Medical, psychological, and welfare support
- Discharge and reunification with sponsors

Although daily operational data is available, a centralized analytical framework is needed to continuously assess:

- Total care system load
- Balance between inflow and outflow
- Capacity stress and relief periods
- Sustainability of care delivery over time

This project addresses these requirements through structured time-series analysis and derived healthcare capacity indicators.

---

## Objectives

### Primary Objectives

- Quantify daily and cumulative care load across CBP and HHS.
- Identify periods of capacity strain and relief.
- Analyze the balance between intake, transfers, and discharges.

### Secondary Objectives

- Support healthcare staffing and shelter planning.
- Improve situational awareness for policymakers.
- Enable data-driven evaluation of humanitarian response.

---

## Dataset

The project uses daily operational data covering **2023–2025**.

| Column | Description |
|---|---|
| `Date` | Reporting date |
| `Apprehended` | Children apprehended and placed in CBP custody; daily intake volume |
| `CBP_Custody` | Children currently in CBP custody; active CBP care load |
| `Transfers` | Children transferred out of CBP custody; flow into the HHS system |
| `HHS_Care` | Children currently receiving HHS care; active HHS care load |
| `Discharges` | Children discharged from HHS care; sponsor placements |

### Data Coverage

- Original reported days: **720**
- Complete calendar span: **1,075 days**
- Duplicate dates identified: **0**
- Date range: **2023-01-12 to 2025-12-21**

---

## Analytical Workflow

### 1. Data Ingestion & Structuring

- Loaded the daily time-series dataset.
- Standardized column names.
- Converted operational variables to numeric format.
- Converted `Date` to datetime.
- Sorted records chronologically.
- Created a complete daily date index.
- Forward-filled stock variables (`CBP_Custody`, `HHS_Care`).
- Filled missing flow variables (`Apprehended`, `Transfers`, `Discharges`) with zero.

### 2. Data Quality & Validation

The analysis checks:

- Duplicate dates
- Missing dates
- Logical operational constraints:
   - Transfers ≤ CBP custody
   - Discharges ≤ HHS care

- Reporting anomalies through validation flags.

The validation identified **86 anomalous records**, primarily associated with the transfer-vs-CBP-custody constraint. Discharge records satisfied the corresponding validation constraint throughout the dataset.

### 3. Healthcare Capacity Metrics

Derived metrics include:

- **Total System Load** = CBP Custody + HHS Care
- **Net Daily Intake** = Transfers − Discharges
- **Care Load Growth Rate** = Day-over-day percentage change in total system load
- **7-Day Net Intake** = 7-day rolling average of net intake
- **Backlog Indicator** = Positive 7-day net intake condition
- **Cumulative Net Intake** = Cumulative balance of transfers and discharges
- **CBP-to-HHS Ratio** = CBP Custody / HHS Care

### 4. Trend & Temporal Analysis

The EDA examines:

- Daily care-load trends
- Weekly averages and peaks
- Monthly averages and peaks
- Sustained high-load periods
- Early-vs-late timeline comparison
- Changes in system pressure over time

### 5. Pressure & Stress Identification

To distinguish short-term fluctuations from sustained capacity pressure, the analysis uses:

- 7-day rolling averages
- 14-day rolling averages
- 7-day rolling standard deviation
- 80th-percentile stress thresholds
- Consecutive stress-day tracking
- Prolonged strain classification for 7+ consecutive stress days

---

# Key Outcomes & Findings

The EDA produced several operationally relevant findings from the 2023–2025 time series.

### 1. Significant Reduction in HHS Care Load

The **peak HHS care load reached 11,516 children**.

The average HHS care load during the earlier portion of the timeline was approximately **8,390 children**, compared with approximately **3,777 children** during the later portion, representing an approximate **55% reduction**.

The most active month identified was **December 2023**, with an average HHS care load of approximately **11,080 children** and a peak of **11,516**.

### 2. High-Load Conditions Were Sustained

A 30-day rolling analysis identified a high-load threshold of approximately **8,704 children**.

The longest sustained high-load period lasted **162 consecutive days**, indicating that elevated system demand was not limited to isolated daily spikes.

### 3. Inflow and Outflow Were Uneven Over Time

Across the analyzed period:

- **238 days** showed inflow greater than outflow, indicating expansion pressure.
- **475 days** showed outflow greater than inflow, indicating relief periods.
- **362 days** were balanced.

The overall average `Net_Daily_Intake` was approximately __-30 children/day__, indicating that transfers minus discharges were negative on average over the full dataset.

### 4. Capacity Stress Was Concentrated in Extended Periods

The 7-day rolling HHS care stress threshold was approximately **8,393 children**, while the 14-day threshold was approximately **8,388 children**.

The analysis identified **214 days above the 7-day stress threshold** and **196 days classified as prolonged strain** under the project's 7+ consecutive stress-day definition.

### 5. System-Wide Peak Responsibility

The maximum combined **CBP + HHS system load was 11,762 children**.

This provides a broader measure of system responsibility than HHS care load alone and can be used to monitor the full care pipeline.

### 6. Operational Variability Was Measurable

The average 7-day rolling standard deviation of HHS care load was approximately **102 children**, providing a baseline indicator of short-term care-load variability.

---

## KPI Outcomes

| KPI | Result |
|---|---:|
| **Peak Children in HHS Care** | **11,516** |
| **Peak Total System Load** | **11,762** |
| **Net Intake Pressure (7-day average)** | **+54** |
| **Care Load Volatility** | **102** |
| **Weekly Backlog Rate** | **376 children/week** |
| **Discharge Effectiveness** | **1.6%** |

These KPIs provide a compact operational view of care demand, pressure, variability, and discharge capacity.

---

## Planned Streamlit Dashboard

The dashboard is planned as the visualization layer for the completed analytical work.

### Core Modules

1. **System Load Overview**

   - Total system load
   - HHS care load
   - CBP custody load
   - Load trends over time

2. **CBP vs HHS Load Comparison**

   - Compare active CBP and HHS care loads
   - Visualize changes in the care pipeline

3. **Net Intake & Backlog Trends**

   - Inflow vs outflow
   - Net intake pressure
   - Cumulative backlog indicators
   - Sustained pressure periods

4. **KPI Summary Cards**

   - Peak children under care
   - Net intake pressure
   - Care load volatility
   - Backlog accumulation
   - Discharge effectiveness

### Planned User Controls

- Date range selector
- Metric toggles
- Time-granularity filters

---

## Technologies Used

- **Python**
- **Pandas** — data manipulation and time-series analysis
- **NumPy** — numerical computations
- **Matplotlib** — data visualization
- **Seaborn** — exploratory visualization
- **Jupyter Notebook** — analysis environment
- **Streamlit** — planned interactive dashboard

---

## Project Structure

```text
Healthcare-Capacity-Care-Load-Analytics/
│
├── Problem Statement (Project 1)(1).txt
├── Project 1.ipynb
├── HHS_Unaccompanied_Alien_Children_Program.csv
├── README.md
│
└── dashboard/
    └── app.py                 # Planned Streamlit application
```

---

## Project Status

- [x] Data ingestion and structuring
- [x] Data quality and validation
- [x] Healthcare capacity metric engineering
- [x] Trend and temporal analysis
- [x] Pressure and stress identification
- [x] KPI development
- [x] Exploratory Data Analysis
- [x] Outcome and insight analysis
- [x] Executive summary
- [ ] Streamlit dashboard

---

## Conclusion

The analysis establishes a structured framework for monitoring **care load, system pressure, pipeline balance, volatility, and sustained capacity stress** across the UAC care system.

The results show a substantial reduction in HHS care load between the earlier and later portions of the study period, while also identifying extended periods of elevated demand and measurable operational stress. These findings provide the analytical foundation for an interactive dashboard that can support monitoring, planning, and data-driven evaluation of system capacity.

## Disclaimer

This project is an analytical and educational implementation based on the provided dataset and problem statement. The derived metrics, thresholds, and stress classifications are analytical constructs developed for this project and should not be interpreted as official HHS capacity limits, policy thresholds, or operational directives.
