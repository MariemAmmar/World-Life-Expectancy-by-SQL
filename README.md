# 🌍 World Life Expectancy — SQL Data Analysis Project

An end-to-end SQL project that cleans, transforms, and explores a global dataset of health and socioeconomic indicators across ~185 countries spanning 16 years (2007–2022).

---

## 📌 Project Overview

This project investigates what factors influence life expectancy around the world. Using MySQL, the raw dataset is first cleaned to resolve duplicates and missing values, then analyzed to uncover relationships between life expectancy and indicators such as GDP, BMI, adult mortality, vaccination rates, and development status.

---

## 📁 Repository Structure

```
├── WorldLifeExpectancy.csv                         # Raw dataset
├── WorldLifeExpectancyScript.sql                   # Table creation & data import
├── 13_1_World_Life_Expectancy_Data_Cleaning.sql    # Data cleaning queries
└── 13_2_World_Life_Expectancy_Exploratory_Data_Analysis.sql  # EDA queries
```

---

## 🗄️ Dataset

| Field | Description |
|---|---|
| `Country` | Country name |
| `Year` | Year of record (2007–2022) |
| `Status` | Developing / Developed |
| `Life expectancy` | Average life expectancy (years) |
| `Adult Mortality` | Adult deaths per 1,000 population |
| `infant deaths` | Infant deaths per 1,000 births |
| `GDP` | GDP per capita (USD) |
| `BMI` | Average national BMI |
| `Schooling` | Average years of schooling |
| `HIV/AIDS` | HIV/AIDS-related deaths per 1,000 |
| `Polio` | Polio immunization coverage (%) |
| `Diphtheria` | Diphtheria immunization coverage (%) |
| `thinness 1-19 years` | Prevalence of thinness in youth (%) |
| `percentage expenditure` | Health expenditure as % of GDP |

~2,941 rows across ~185 countries.

---

## 🧹 Part 1 — Data Cleaning

**File:** `13_1_World_Life_Expectancy_Data_Cleaning.sql`

### Steps performed:

**1. Duplicate Detection & Removal**
- Identified duplicate rows using `CONCAT(Country, Year)` as a composite key
- Used `ROW_NUMBER() OVER (PARTITION BY ...)` to flag duplicates
- Deleted duplicate `Row_ID` values, keeping only the first occurrence

**2. Filling Missing `Status` Values**
- Found rows with blank `Status` (Developing / Developed)
- Used self-joins to populate blank status values based on other records from the same country
- Applied separately for both `'Developing'` and `'Developed'` classifications

**3. Imputing Missing `Life expectancy` Values**
- Identified rows with blank life expectancy
- Imputed missing values by averaging the previous and next year's values for the same country using a triple self-join
- Rounded results to 1 decimal place

---

## 🔍 Part 2 — Exploratory Data Analysis

**File:** `13_2_World_Life_Expectancy_Exploratory_Data_Analysis.sql`

### Key analyses:

**Life Expectancy Trends by Country**
- Calculated the min, max, and 15-year improvement in life expectancy per country
- Identified which countries made the most and least progress over the period

**Global Life Expectancy Over Time**
- Computed the average global life expectancy per year to observe worldwide trends

**GDP vs. Life Expectancy**
- Ranked countries by average GDP and compared with average life expectancy
- Split countries into high-GDP (≥ $1,500) and low-GDP (≤ $1,500) groups:
  - High GDP → higher average life expectancy
  - Low GDP → significantly lower average life expectancy

**Development Status vs. Life Expectancy**
- Compared average life expectancy between Developed and Developing nations
- Also counted the number of distinct countries in each category for context

**BMI vs. Life Expectancy**
- Ranked countries by average BMI and compared against life expectancy
- Revealed correlations between nutritional status and longevity

**Rolling Adult Mortality**
- Computed a running total of adult mortality over time, partitioned by country
- Demonstrated window function usage for temporal trend tracking
- Filtered to countries matching `'%United%'` as a focused example

---

## 🛠️ Tools & Technologies

- **Database:** MySQL
- **Language:** SQL
- **Techniques:** CTEs, subqueries, window functions (`ROW_NUMBER`, `SUM OVER`), self-joins, conditional aggregation (`CASE WHEN`), `GROUP BY`, `HAVING`

---

## 💡 Key Findings

- Countries with **higher GDP** tend to have a life expectancy roughly **10+ years longer** than low-GDP countries.
- **Developed nations** average notably higher life expectancy than developing ones, though the developing group contains far more countries.
- Several nations showed **dramatic improvements** (15+ years) in life expectancy over the study period, particularly in Sub-Saharan Africa.
- **Adult mortality** and **HIV/AIDS** rates show strong inverse relationships with life expectancy.
- **Schooling years** and life expectancy show a clear positive correlation across countries.

---

## 🚀 Getting Started

1. Clone this repository
2. Run `WorldLifeExpectancyScript.sql` to create and populate the table
3. Run the data cleaning script: `13_1_World_Life_Expectancy_Data_Cleaning.sql`
4. Run the EDA script: `13_2_World_Life_Expectancy_Exploratory_Data_Analysis.sql`

> Tested on **MySQL 8.0+**. Window functions require MySQL 8.0 or later.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
