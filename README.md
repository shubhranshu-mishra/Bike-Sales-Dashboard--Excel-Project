# Bike Sales Dashboard (Excel Data Analysis Project)

### Data Source- 
https://github.com/AlexTheAnalyst/Excel-Tutorial/blob/main/Excel%20Project%20Dataset.xlsx

### Given- 
Given data is kept as raw data in the first sheet

### Procedure- 
- At first filter was applied on the dataset
- Duplicates were removed from the dataset
- A new column age bracket was created using nested IF statement and 3 age brackets were identified (formed)
  - Adolescent: Beloww 30 years
  - Middle age: Between 30 to 60 years
  - Old: Above 60 years
- 3 Pivot table was created followed by 3 pivot charts based on need
- A dashboard is created in a new sheet and pivot chart is placed in specific location
- Slicer were placed as per the requirement for data analysis and placed in a specific location

### Done by Shubhranshu Mishra (Github: shubhranshu-mishra)
-------------------------------------------------------------------------------------------------------------------------------
# Comprehensive Data Case Study: Bike Consumer Purchasing Analytics

## Executive Summary
This project outlines the end-to-end data lifecycle—spanning raw data ingestion, structural validation, cleaning, feature engineering, and dimensional modeling—to build an interactive operations dashboard in Microsoft Excel. 

The objective was to identify core purchasing patterns within a sample population of 1,000 prospective bike buyers. By aggregating multi-layered demographic parameters (income metrics, geographical location, education, and daily commute constraints), this dashboard surfaces targeted consumer personas to help optimize marketing spend and inventory distribution for a bicycle retail operations model.

---

## 1. Data Source & Initial Quality Assessment
The raw dataset consists of a flat table containing demographic and behavioral profiles of unique individuals. An initial audit of the data schema revealed several structural issues, formatting inconsistencies, and analytical noise that required resolution before any visualization could take place.

### Raw Data Dictionary
*   **ID**: Unique identifier assigned to each prospective customer.
*   **Marital Status / Gender**: Single-character strings (`M`/`S`, `M`/`F`).
*   **Income**: Numeric values representing annual salary.
*   **Children / Cars**: Integer counts.
*   **Education / Occupation**: Categorical strings representing socioeconomic tiers.
*   **Home Owner**: Boolean flag indicating property ownership (`Yes`/`No`).
*   **Commute Distance**: Ranged text fields tracking physical distance to workplace.
*   **Region**: Categorical fields tracking global territories (*North America*, *Europe*, *Pacific*).
*   **Age**: Continuous numerical integer.
*   **Purchased Bike**: Target classification variable indicating conversion (`Yes`/`No`).

---

## 2. Data Cleaning & Engineering Lifecycle
To preserve the integrity of the original dataset, a strict workflow isolation policy was maintained: the raw source data was locked, and all transformations were executed on an independent staging worksheet.

[Raw Sheet] ──> [Staging Sheet: Cleaning & Transformation] ──> [Pivot Sheet] ──> [Dashboard UI]

### Step 1: De-duplication
A global scan across all field combinations identified multiple duplicate rows sharing identical structural properties and IDs. These records were purged to ensure that average salary evaluations and conversion counts were not artificially inflated.

### Step 2: Attribute Standardization
Raw entries in the `Marital Status` and `Gender` columns relied on single-letter abbreviations. While computationally efficient, these fields lack clarity for downstream business users. 
*   Column `B` (`Marital Status`) values were explicitly mapped: `M` -> `Married`, `S` -> `Single`.
*   Column `C` (`Gender`) values were explicitly mapped: `M` -> `Male`, `F` -> `Female`.

*Note: The find-and-replace pipeline was executed on a strict column-by-column basis to prevent logical collision (e.g., preventing the `M` in Gender from converting into "Married").*

### Step 3: Feature Engineering (Algorithmic Age Bucket Brackets)
Plotting raw, continuous ages (ranging from 25 to 89) directly onto charts generates extreme visual noise, rendering trend lines unreadable. To extract clean demographic trends, a new categorical dimension—`Age Brackets`—was engineered using a nested logic function:

`=IF(L2>54, "Old", IF(L2>=31, "Middle Age", IF(L2<31, "Adolescent", "Invalid")))`

This structural change segmented the population into three distinct, high-level groups:
*   **Adolescent**: Under 31 years old
*   **Middle Age**: 31 to 54 years old
*   **Old**: 55 years and older

### Step 4: Normalization of Sorting Anomalies
During exploratory chart generation, the `Commute Distance` dimension triggered a logical sorting bug. Excel natively ordered the categories alphabetically rather than sequentially, forcing the `"10+ Miles"` category directly between `"1-2 Miles"` and `"2-5 Miles"`. 

To fix this visual error, the text strings in the staging sheet were uniformly rewritten (e.g., transforming `"10+ Miles"` into a standardized text block `"More than 10 miles"`), allowing the charts to plot a clean, linear progression of distance.

---

## 3. Dimensional Data Modeling via Pivot Tables
Once normalized, the staging data was modeled into three distinct analytical layers using isolated pivot tables. Each table isolates specific demographic intersections against the primary KPI: customer conversion (`Purchased Bike`).

### Matrix 1: Financial Profile vs. Conversion by Gender
*   **Dimension Blocks**: `Gender` (Rows), `Purchased Bike` (Columns).
*   **Value Aggregation**: `Average of Income`.
*   **Key Insight**: A clear income gap exists between converting and non-converting customers. On average, individuals who purchased a bike earned roughly $5,000 to $8,000 more annually than those who opted out. Additionally, male consumers demonstrated both a higher average income ceiling and a higher conversion volume within this specific dataset.

### Matrix 2: Logistics Layer (Commute Impact)
*   **Dimension Blocks**: `Commute Distance` (Rows), `Purchased Bike` (Columns).
*   **Value Aggregation**: `Count of Purchased Bike`.
*   **Key Insight**: Bicycles are predominantly purchased by customers with localized commutes. The highest volume of conversions occurs among individuals traveling between 0–1 miles to work, with sharp drops in conversion as the commute distance expands past 5 miles.

### Matrix 3: Demographic Lifecycle Layer
*   **Dimension Blocks**: `Age Brackets` (Rows), `Purchased Bike` (Columns).
*   **Value Aggregation**: `Count of Purchased Bike`.
*   **Key Insight**: The core consumer segment is heavily concentrated in the "Middle Age" (31–54) bracket. Both "Adolescents" and "Old" demographics showed lower conversion metrics, establishing the exact middle-aged market as the primary target for regional ad spend.

---

## 4. UI/UX Dashboard Architecture & Interactivity
The final presentation layer converts these analytical matrices into a unified, interactive decision-making tool.

### Design Implementations
*   **Gridline Suppression**: Standard spreadsheet lines were disabled via UI view controls, transitioning the workspace from a raw data sheet into a clean application interface.
*   **Coordinated Cross-Filtering (Slicers)**: Dedicated, stylized UI Slicers were generated for three external dimensions: `Marital Status`, `Region`, and `Education Level`. 
*   **Report Connections Pipeline**: Using Excel's backend connection architecture, these slicers were linked globally to all underlying pivot tables simultaneously. Selecting a single filter block—such as *Single* + *Europe* + *Bachelors Degree*—instantly forces all three charts to re-render in real-time, allowing stakeholders to run hyper-specific demographic queries instantly.
