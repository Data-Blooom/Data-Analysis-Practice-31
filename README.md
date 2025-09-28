# 🌍 International Student Population Trends Analysis

A comprehensive data analysis project exploring trends and composition of international student populations across academic years.

## 📊 Project Overview
This Power BI dashboard analyzes international student data, tracking demographic trends, geographic distribution, and year-over-year changes. The project provides valuable insights for educational institutions' strategic planning and recruitment strategies.

## 🎯 Key Insights
- **Total Students:** 256,000
- **Female Student Share:** 72.44%
- **Dominant Nationality Group:** G.C.C (Gulf Cooperation Council Countries)
- **Steady YoY Growth:** From 24K (2016/2017) to 38K (2022/2023)
- **Wide Geographic Reach:** Students from 52+ countries across all continents

## 📈 Visualizations Included
1. **Student Balance by Nationality Groups** (Annual trends)
2. **Gender Share Evolution** (Year-over-year comparison)
3. **YoY Change in Student Numbers** (Growth analysis)
4. **World Map Distribution** (Geographic origin visualization)

## 🔧 Technical Implementation

### Data Model Structure
- **Fact Table:** 'Public Colleges' with student records
- **Key Columns:** Year, Nationality, Gender, Number of Students
- **Calculated Columns:** StartYear for time intelligence

### DAX Measures & Calculations

```dax
-- Total Students Calculation
Total Students = SUM('Public Colleges'[Number of Students])

-- Female Student Percentage
Female Share = 
VAR Total = SUM('Public Colleges'[Number of Students])
VAR Female = CALCULATE(
    SUM('Public Colleges'[Number of Students]),
    'Public Colleges'[Gender] = "زن"
)
RETURN DIVIDE(Female, Total, 0) * 100

-- Dominant Nationality Group Identification
Dominant Nationality Group = 
VAR TopRow = TOPN(
    1,
    SUMMARIZE(
        'Public Colleges',
        'Public Colleges'[Nationality],
        "Total", SUM('Public Colleges'[Number of Students])
    ),
    [Total], DESC
)
RETURN CONCATENATEX(TopRow, 'Public Colleges'[Nationality], ", ")

-- Latest Year Student Count
Students Latest Year = 
VAR MaxYear = MAX('Public Colleges'[Year])
RETURN CALCULATE(
    SUM('Public Colleges'[Number of Students]),
    'Public Colleges'[Year] = MaxYear
)

-- Year-over-Year Change Calculation
YoY Change = 
VAR CurrentYear = MAX('Public Colleges'[StartYear])
VAR CurrentTotal = CALCULATE(
    SUM('Public Colleges'[Number of Students]),
    'Public Colleges'[StartYear] = CurrentYear
)
VAR PrevTotal = CALCULATE(
    SUM('Public Colleges'[Number of Students]),
    'Public Colleges'[StartYear] = CurrentYear - 1
)
RETURN CurrentTotal - PrevTotal

-- StartYear Extraction for Time Intelligence
StartYear = VALUE(LEFT('Public Colleges'[Year], 4))

-- Transparent Background for Visuals
Background Removal = "rgba(255, 255, 255, 0)"
