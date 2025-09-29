# 🌍 Project Documentation: International Student Population Analysis  

---

## 📌 Project Overview and Purpose  
This document provides the technical and analytical documentation for the **International Student Population Trend and Composition Analysis** dashboard, covering the academic years **2014/2015 through 2022/2023**.  

Developed entirely in **Power BI**, this project's primary objective was to move beyond simple reporting and transform raw enrollment data into a strategic, decision-making tool.  

The dashboard offers:  
- Critical visibility into demographic trends  
- Sustained growth trajectories  
- Performance of key international source markets  

These insights inform strategic planning for resource allocation and recruitment.  

---

## 🗂️ Data Architecture, Modeling, and Preparation  

### 1️⃣ Data Source and Tooling  
- Core data originates from a single enrollment table, designated as **'Public Colleges'** in the Power BI Data Model.  
- All complex calculations and time-intelligence analyses are driven by **DAX** (Data Analysis Expressions) measures.  

### 2️⃣ Robust ETL and Data Cleaning (Power Query)  
Key steps executed in **Power Query**:  
- **Junk Removal**: Removed all blank rows and columns to streamline dataset and prevent errors.  
- **Dimensional Consolidation**: Eliminated redundant/irrelevant columns, following **star schema** best practices.  
- **Value Correction**: Standardized categorical fields (e.g., nationality names, gender labels).  

### 3️⃣ Calculated Columns for Time Intelligence  
Created a numerical calculated column `StartYear` for YoY analysis:  

```DAX
StartYear = VALUE( LEFT ( 'Public Colleges'[Year], 4 ) )

YoY Change = 
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


Total Students = SUM('Public Colleges'[Number of Students])


Latest Year Students = 
VAR MaxYear = MAX('Public Colleges'[Year])
RETURN
    CALCULATE(
        SUM('Public Colleges'[Number of Students]),
        'Public Colleges'[Year] = MaxYear
    )


Female Student Share = 
VAR Total = SUM('Public Colleges'[Number of Students])
VAR Female = CALCULATE(
    SUM('Public Colleges'[Number of Students]),
    'Public Colleges'[Gender] = "Female"
)
RETURN
    DIVIDE(Female, Total, 0) * 100


Dominant Nationality Group = 
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


```

## 📊 Key Strategic Insights

| Key Performance Indicator              | Value  | Strategic Interpretation                                                                 |
| -------------------------------------- | ------ | ---------------------------------------------------------------------------------------- |
| **Total Cumulative Students**          | 256K   | The total enrollment base analyzed over the entire period.                               |
| **Latest Year Enrollment (2022/2023)** | 38K    | Confirms strong, sustained student attraction and enrollment growth.                     |
| **Female Student Share**               | 72.44% | Indicates a profound gender dominance that must inform planning and program development. |
| **Dominant Nationality Group**         | G.C.C. | Reinforces Gulf Cooperation Council nations as the critical source market.               |


## 📊 Visuals Used

- Annual Enrollment Trend Chart → Shows growth performance over time.

- Gender Composition by Year → Annual breakdown of Male vs. Female.

- Nationality Enrollment Trend (Stacked Bar) → Market share by nationality groups (G.C.C., Other Countries, Rest of Arab World).

- Geographic Distribution Map → Visualizes spread of student origins.


## ✅ Conclusion

This Power BI project demonstrates the impact of structured data preparation and advanced DAX modeling in transforming complex educational data into clear strategic guidance.

## 🔑 Key findings:

- 72.44% female dominance across the student population.

- G.C.C. group as the dominant nationality source.

- Sustained enrollment growth supporting internationalization strategies.
