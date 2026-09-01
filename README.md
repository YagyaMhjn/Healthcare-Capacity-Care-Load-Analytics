# Healthcare Capacity & Care Load Analytics

## Project Overview

This project analyzes daily operational data from the **Unaccompanied Alien Children (UAC) Program** to evaluate healthcare system capacity, care load, and the balance between inflows and outflows across the CBP and HHS care pipeline.

The analysis uses daily time-series data covering **2023–2025** and transforms operational counts into healthcare capacity metrics, trend indicators, backlog measures, and stress signals. The objective is to support better situational awareness and identify periods of sustained capacity pressure rather than relying on reactive decision-making.

> **Current status:** Exploratory Data Analysis (EDA) and analytical metric development are completed. The Streamlit dashboard is planned as the next stage and has not yet been implemented.

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

The project uses daily operational data for the period **2023–2025**.

| Column | Description |
|---|---|
| `Date` | Reporting date |
| `Apprehended` | Children apprehended and placed in CBP custody; daily intake volume |
| `CBP_Custody` | Children currently in CBP custody; active CBP care load |
| `Transfers` | Children transferred out of CBP custody; flow into the HHS system |
| `HHS_Care` | Children currently receiving HHS care; active HHS care load |
| `Discharges` | Children discharged from HHS care; sponsor placements |

---

## Analytical Workflow

### 1. Data Ingestion & Structuring

- Loaded the daily time-series dataset.
- Standardized column names.
- Converted numerical fields into usable numeric formats.
- Converted `Date` to datetime.
- Sorted records chronologically.
- Created a complete daily date index.
- Forward-filled stock variables (`CBP_Custody`, `HHS_Care`) and set missing flow variables to zero.

### 2. Data Quality & Validation

The analysis checks:

- Duplicate dates
- Missing dates
- Logical operational constraints:
   - Transfers ≤ CBP custody
   - Discharges ≤ HHS care

- Reporting anomalies through validation flags.

### 3. Healthcare Capacity Metrics

The following derived metrics were created:

- **Total System Load** = CBP Custody + HHS Care
- **Net Daily Intake** = Transfers − Discharges
- **Care Load Growth Rate** = Day-over-day percentage change in total system load
- **Backlog Indicator** = Sustained positive 7-day average net intake
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
- Prolonged strain periods defined as **7+ consecutive stress days**

---

## Key Performance Indicators

The project defines five core KPIs:

| KPI | Purpose |
|---|---|
| **Total Children Under Care** | Measures overall system responsibility |
| **Net Intake Pressure** | Measures inflow/outflow imbalance |
| **Care Load Volatility Index** | Measures stability of the care load |
| **Backlog Accumulation Rate** | Indicates sustained care pressure |
| **Discharge Offset Ratio** | Measures the ability of discharges to relieve system load |

---

## EDA & Insights

The EDA produces an executive-level view of:

### Total Care System Load

Measures the number of children under CBP and HHS responsibility and identifies peak and average care-load periods.

### Inflow vs Outflow Balance

Compares transfers into HHS care with discharges to identify expansion pressure and relief periods.

### Capacity Stress

Uses rolling averages and percentile-based thresholds to identify periods where care load remains unusually high for sustained periods.

### Operational Stability

Uses rolling volatility to identify periods of high fluctuation in HHS care load.

### Backlog Pressure

Tracks cumulative net intake and sustained positive intake conditions as indicators of accumulating system pressure.

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

   - Total children under care
   - Net intake pressure
   - Care load volatility
   - Backlog accumulation
   - Discharge offset ratio

### Planned User Controls

- Date range selector
- Metric toggles
- Time-granularity filters

---

## Technologies Used

- **Python**
- **Pandas** — data manipulation and time-series analysis
- **NumPy** — numerical computations
- **Matplotlib** — visualization
- **Seaborn** — exploratory visualization
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

## Deliverables

- [x] Data ingestion and structuring
- [x] Data quality and validation
- [x] Healthcare capacity metric engineering
- [x] Trend and temporal analysis
- [x] Pressure and stress identification
- [x] KPI development
- [x] Exploratory Data Analysis
- [x] Executive summary
- [ ] Streamlit dashboard

---

## Conclusion

This project converts daily UAC operational data into a structured analytical framework for monitoring **care load, system pressure, pipeline balance, volatility, and sustained capacity stress**. The completed EDA establishes the analytical foundation for an interactive Streamlit dashboard intended to provide a consolidated view of system conditions and support data-driven operational planning.

## Disclaimer

This project is an analytical and educational implementation based on the provided dataset and problem statement. The derived metrics and thresholds are analytical constructs for monitoring and should not be interpreted as official HHS capacity limits or policy thresholds.
