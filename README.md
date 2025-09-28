# International Student Population Analysis 📊

**Project Overview:**
This project involved a comprehensive data analysis and visualization, implemented via **Power BI**, to analyze the trend and composition of the international student population over multiple academic years (2014/2015 to 2022/2023). The analysis transforms raw student data into actionable insights regarding demographics, nationality market share, and enrollment growth trajectory.

---

## 🔑 Key Performance Indicators (KPIs)

| Metric | Value | Insight |
| :--- | :--- | :--- |
| **Total Cumulative Students** | **256K** | The total volume of international students analyzed across the full period. |
| **Students in Latest Year (2022/2023)** | **38K** | Demonstrates the peak annual enrollment capacity achieved in the latest cycle. |
| **Share of Female Students** | **72.44%** | A significant gender imbalance, indicating a high rate of female participation. |
| **Dominant Nationality Group** | **G.C.C. (Gulf Cooperation Council)** | Confirms the primary source market for international student recruitment. |

---

## 📈 Key Findings & Data Insights

1.  **Sustained Growth Trajectory:** The total annual student count has shown continuous **Year-over-Year (YoY) growth**, peaking at **38K** students in the latest academic cycle (2022/2023). This trend confirms the success of long-term international recruitment strategies.
2.  **Gender Skew:** Female students significantly dominate the population with an overall share of **72.44%**. In the latest year (2022/2023), the enrollment split was approximately **26K Female** vs. **12K Male** students.
3.  **Market Concentration:** The **Gulf Cooperation Council (G.C.C.)** countries consistently contribute the largest segment of the international student body. This concentration highlights both the strength of this market and the strategic need for diversification into other growing regions.
4.  **Geographic Diversity:** The distribution map confirms student origins from diverse global regions, including North America, Europe, Asia, and Africa, identifying specific source countries such as Egypt, Yemen, Pakistan, and the UK.

---

## ⚙️ Power BI / DAX Measures Implementation

The following Data Analysis Expressions (DAX) were used to calculate the core KPIs and metrics displayed on the dashboard, using the dimensional table `'Public Colleges'`.

### 1. Total Students & Enrollment Metrics

| Measure Name | DAX Formula | Description |
| :--- | :--- | :--- |
| **تعداد کل دانشجویان** | ```DAX
SUM('Public Colleges'[Number of Students])
``` | Calculates the total cumulative number of students across all records. |
| **تعداد دانشجویان سال آخری** | ```DAX
VAR MaxYear = MAX('Public Colleges'[Year])
RETURN
    CALCULATE(
        SUM('Public Colleges'[Number of Students]),
        'Public Colleges'[Year] = MaxYear
    )
``` | Calculates the total student count for the most recent academic year available in the data. |
| **YoY Change** | ```DAX
VAR CurrentYear = MAX('Public Colleges'[StartYear])
VAR CurrentTotal =
    CALCULATE(
        SUM('Public Colleges'[Number of Students]),
        'Public Colleges'[StartYear] = CurrentYear
    )
VAR PrevTotal =
    CALCULATE(
        SUM('Public Colleges'[Number of Students]),
        'Public Colleges'[StartYear] = CurrentYear - 1
    )
RETURN
    CurrentTotal - PrevTotal
``` | Calculates the Year-over-Year absolute change in student numbers based on the calculated `StartYear`. |
| **StartYear (Calculated Column)** | ```DAX
VALUE( LEFT ( 'Public Colleges'[Year], 4 ) )
``` | Extracts the starting year (e.g., 2022 from 2022/2023) to be used for time intelligence calculations. |

### 2. Gender Composition

| Measure Name | DAX Formula | Description |
| :--- | :--- | :--- |
| **سهم دانشجویان زن** | ```DAX
VAR Total = SUM('Public Colleges'[Number of Students])
VAR Female = CALCULATE(
    SUM('Public Colleges'[Number of Students]),
    'Public Colleges'[Gender] = "زن"
)
RETURN
    DIVIDE(Female, Total, 0) * 100
``` | Calculates the percentage share of female students in the overall student population. |

### 3. Nationality Analysis

| Measure Name | DAX Formula | Description |
| :--- | :--- | :--- |
| **غالب ترین گروه ملیتی** | ```DAX
VAR TopRow =
    TOPN(
        1,
        SUMMARIZE(
            'Public Colleges',
            'Public Colleges'[Nationality],
            "Total", SUM('Public Colleges'[Number of Students])
        ),
        [Total], DESC
    )
RETURN
CONCATENATEX(TopRow, 'Public Colleges'[Nationality], ", ")
``` | Identifies and returns the name of the nationality group with the single highest total student count across all years. |

### 4. Visualization Settings

| Setting | Value | Purpose |
| :--- | :--- | :--- |
| **حذف بک گراند (Transparent Background)** | `"rgba(255, 255, 255, 0)"` | Used to set a transparent background for visuals within Power BI for clean dashboard integration. |
