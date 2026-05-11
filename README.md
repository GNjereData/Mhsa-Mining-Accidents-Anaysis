#  MSHA Mining Accidents Analysis (2000–2024)

A full end-to-end data analysis project using **Excel (Power Query)**, **SQL (SQLite)**, and **Power BI** — analyzing over 272,000 mining accident records from the U.S. Mine Safety and Health Administration (MSHA).

---

## 📁 Project Files

| File | Tool | Description |
|------|------|-------------|
| `MSHA_Accidents_Cleaned.xlsx` | Excel / Power Query | Cleaned dataset + pivot table analysis |
| `MSHA_Accidents_SQL.sqbpro` | SQLite / DB Browser | SQL queries and database project file |
| `MSHA_Accidents_Dashboard.pbix` | Power BI | Interactive dashboard with 5 visuals |

>  **Raw Data Source:** The original dataset (`MSHA_Accidents_Raw.csv`) is publicly available from the [U.S. Mine Safety and Health Administration](https://www.msha.gov/data-and-reports/mine-data-retrieval-system) — 272,149 records, 57 columns.

---

##  Dataset Overview

| Stat | Value |
|------|-------|
| Total Records | 272,149 |
| Columns | 57 |
| Date Range | 2000 – 2024 |
| Unique Mine Sites | 13,434 |
| Unique Operators | 10,142 |
| U.S. States & Territories | 54 |
| Total Fatalities | 1,197 |
| Total Days Lost to Injury | 6,247,505 |
| Metal Mining Records | 144,748 (53%) |
| Coal Mining Records | 127,401 (47%) |

---

## Tools & Skills Used

- **Excel & Power Query** — data cleaning, time formatting, transformation, pivot tables
- **SQL (SQLite)** — aggregations, CASE WHEN bucketing, HAVING filters, GROUP BY
- **Power BI** — interactive dashboard with 5 chart types and slicers

---

## Data Cleaning (Power Query)

The raw dataset required significant cleaning before analysis:

- Replaced `9999` placeholder values in `ACCIDENT_TIME` and `SHIFT_BEGIN_TIME` with `0` (~20,000 and ~8,400 rows respectively)
- Formatted time columns from raw integers (e.g. `1815`, `830`, `15`) into readable `HH:MM` format using `Text.PadStart` and `Text.Insert`
- Fixed mixed date formats in `ACCIDENT_DT` — some rows stored as datetime, others as plain text strings
- Trimmed whitespace from all text columns (operator names, occupation, narrative)
- Set correct data types across all 57 columns
- Removed `CONTRACTOR_ID` column (~90% empty, not useful for analysis)

---

## ❓ Analysis Questions & Key Insights

### Q1 — What time of day has the most accidents?

| Time Window | Accidents | Share |
|-------------|-----------|-------|
| Morning (6am – Noon) | 99,090 | 36% |
| Afternoon (Noon – 6pm) | 77,751 | 29% |
| Evening (6pm – Midnight) | 63,695 | 23% |
| Night (Midnight – 6am) | 31,601 | 12% |

> **Morning shift is the most dangerous window**, accounting for nearly 36% of all incidents. Daytime hours combined (6am–6pm) represent ~65% of all accidents — reflecting peak operational activity as the primary risk driver. Night shifts still recorded over 31,000 incidents, showing no shift is without risk.

**Accidents by Quarter:**
| Quarter | Accidents |
|---------|-----------|
| Q1 (Jan–Mar) | 68,730 |
| Q2 (Apr–Jun) | 68,786 |
| Q3 (Jul–Sep) | 74,207 |
| Q4 (Oct–Dec) | 60,426 |

> Q3 (summer months) sees the highest accident volume — Q4 the lowest.

---

### Q2 — Which 5 operators have the most fatalities?

| Operator | Fatalities | Total Accidents | Days Lost | Avg Experience |
|----------|-----------|----------------|-----------|----------------|
| Performance Coal Company | 31 | 377 | 10,866 | 6.93 yrs |
| Jim Walter Resources Inc | 21 | 2,258 | 70,293 | 5.91 yrs |
| Consolidation Coal Company | 17 | 3,150 | 108,743 | 7.31 yrs |
| Wolf Run Mining Company | 13 | 153 | 3,112 | 5.69 yrs |
| Vulcan Construction Materials LLC | 10 | 2,238 | 52,416 | 7.53 yrs |

> **Performance Coal Company** stands out for its disproportionate fatality density — 31 deaths from only 377 total accidents. Combined days lost across all five operators: **245,430 days**. Average worker experience ranged from 5.7 to 7.5 years, indicating that **fatalities are not concentrated among inexperienced workers** — a key safety insight.

**Fatalities by Mining Type:**
| Type | Fatalities |
|------|-----------|
| Metal Mining | 641 |
| Coal Mining | 556 |

---

### Q3 — Which body parts result in the most days lost? (Min. 100 incidents)

| Rank | Body Part | Incidents | Avg Days Lost |
|------|-----------|-----------|---------------|
| 1 | Shoulders (Collarbone/Clavicle/Scapula) | 13,521 | 55.73 |
| 2 | Multiple Parts (More Than One Major) | 11,483 | 54.24 |
| 3 | Trunk, Multiple Parts | 1,768 | 50.88 |
| 4 | Leg, Multiple Parts | 336 | 50.45 |
| 5 | Lower Extremities, Multiple Parts | 971 | 49.13 |
| 6 | Leg, NEC | 2,872 | 46.45 |
| 7 | Lower Leg/Tibia/Fibula | 4,098 | 44.46 |
| 8 | Neck | 4,105 | 43.66 |
| 9 | Back (Muscles/Spine/Spinal Cord) | 28,156 | 42.62 |
| 10 | Upper Arm/Humerus | 1,683 | 41.58 |

> **Shoulder injuries are the most costly per incident** at 55.73 average days lost. However, **back injuries** rank as the single most frequent injury with 28,156 cases — making them the **largest overall contributor to lost productivity** across the entire industry.

**Overall Days Lost Stats:**
| Stat | Value |
|------|-------|
| Average days lost per incident | 27.7 days |
| Maximum days lost (single incident) | 3,616 days |
| Total industry days lost | 6,247,505 |

---

### Q4 — Which mine subunit has the most accidents?

| Subunit | Accidents |
|---------|-----------|
| Underground | 106,285 |
| Strip, Quarry, Open Pit | 81,038 |
| Mill Operation/Preparation Plant | 67,158 |
| Surface at Underground | 10,565 |
| Dredge | 3,896 |

> **Underground operations** are the most dangerous environment, accounting for 39% of all accidents — despite being among the most heavily regulated mining environments.

---

### Q5 — Days Away from Work vs. Restricted Activity

| Outcome | Count |
|---------|-------|
| Days Away from Work Only | 88,199 |
| Days Restricted Activity Only | 42,984 |

> Workers were **twice as likely** to be sent home entirely than placed on light duty, indicating the severity of injuries sustained in mining operations.

**Full Injury Severity Breakdown:**
| Degree of Injury | Count |
|-----------------|-------|
| Days Away from Work Only | 88,199 |
| No Days Away / No Restriction | 70,900 |
| Days Restricted Activity Only | 42,984 |
| Accident Only | 30,340 |
| Days Away + Restricted Activity | 20,494 |
| Occupational Illness | 10,497 |
| Permanent Disability | 2,457 |
| Fatality | 1,197 |

---

## 📍 Additional Insights

**Top 5 States by Accident Volume:**
| State | Accidents |
|-------|-----------|
| West Virginia | 41,560 |
| Kentucky | 28,696 |
| Pennsylvania | 20,327 |
| Illinois | 12,652 |
| Texas | 11,840 |

**Top 5 Accident Types:**
| Accident Type | Count |
|---------------|-------|
| Over-exertion NEC | 32,884 |
| Struck by... NEC | 31,728 |
| Accident without injuries | 30,340 |
| Struck by falling object | 23,774 |
| Over-exertion in lifting | 17,892 |

**Top 5 Nature of Injuries:**
| Nature of Injury | Count |
|-----------------|-------|
| Sprain / Strain / Ruptured Disc | 75,563 |
| Cut / Laceration / Puncture | 52,678 |
| Fracture / Chip | 34,514 |
| Contusion / Bruise | 18,499 |

**Worker Experience Averages (Industry-Wide):**
| Experience Type | Average |
|----------------|---------|
| Total Career Experience | 10.97 years |
| Experience at Current Mine | 6.47 years |
| Experience in Current Job | 6.89 years |

**Year-over-Year Safety Trend:**
| Year | Accidents |
|------|-----------|
| 2000 | 18,704 |
| 2024 | 5,990 |
| Change | **↓ 68% reduction over 24 years** |

> A 68% reduction in accidents over 24 years reflects significant improvements in mining safety regulations, equipment, and training across the industry.

---

## Power BI Dashboard

The Power BI report includes 5 interactive visuals:

- **Line Chart** — Accidents per year (2000–2024) showing the 68% decline
- **Horizontal Bar Chart** — Accidents by type
- **Column Chart** — Average days lost by injury severity
- **Horizontal Bar Chart** — Accidents by mine subunit
- **Donut Chart** — Days away from work vs. restricted activity

---

## 💾 SQL Highlights

```sql
-- Q1: Time of day bucketing using CASE WHEN
SELECT
    CASE
        WHEN ACCIDENT_TIME >= 600  AND ACCIDENT_TIME < 1200 THEN 'Morning (6am-Noon)'
        WHEN ACCIDENT_TIME >= 1200 AND ACCIDENT_TIME < 1800 THEN 'Afternoon (Noon-6pm)'
        WHEN ACCIDENT_TIME >= 1800 AND ACCIDENT_TIME < 2400 THEN 'Evening (6pm-Midnight)'
        WHEN ACCIDENT_TIME >= 1    AND ACCIDENT_TIME < 600  THEN 'Night (Midnight-6am)'
        ELSE 'Unknown'
    END AS TIME_OF_DAY,
    COUNT(*) AS TOTAL_ACCIDENTS
FROM MSHA_Accidents
GROUP BY TIME_OF_DAY
ORDER BY TOTAL_ACCIDENTS DESC;

-- Q2: Top 5 operators by fatality count
SELECT
    OPERATOR_NAME,
    COUNT(CASE WHEN DEGREE_INJURY = 'FATALITY' THEN 1 END) AS TOTAL_FATALITIES,
    COUNT(*)                                                AS TOTAL_ACCIDENTS,
    SUM(DAYS_LOST)                                          AS TOTAL_DAYS_LOST,
    ROUND(AVG(TOT_EXPER), 2)                               AS AVG_EXPERIENCE_YRS
FROM MSHA_Accidents
GROUP BY OPERATOR_NAME
ORDER BY TOTAL_FATALITIES DESC
LIMIT 5;

-- Q3: Body parts with highest average days lost (min 100 incidents)
SELECT
    INJURY_SOURCE,
    COUNT(*)                   AS TOTAL_INCIDENTS,
    ROUND(AVG(DAYS_LOST), 2)  AS AVG_DAYS_LOST
FROM MSHA_Accidents
WHERE DAYS_LOST IS NOT NULL
  AND INJURY_SOURCE IS NOT NULL
GROUP BY INJURY_SOURCE
HAVING COUNT(*) >= 100
ORDER BY AVG_DAYS_LOST DESC
LIMIT 10;
```

---

How to View This Project

1. **Cleaned Data & Pivot Tables** — open `MSHA_Accidents_Cleaned.xlsx` in Microsoft Excel
2. **SQL Queries** — open `MSHA_Accidents_SQL.sqbpro` in [DB Browser for SQLite](https://sqlitebrowser.org/) (free)
3. **Dashboard** — open `MSHA_Accidents_Dashboard.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
4. **Raw Data** — download directly from [MSHA's public database](https://www.msha.gov/data-and-reports/mine-data-retrieval-system)

---

 About

This project was completed as part of a data analytics portfolio, demonstrating end-to-end skills across data cleaning, SQL querying, and dashboard design — applied to real-world workplace safety data from the U.S. mining industry.
