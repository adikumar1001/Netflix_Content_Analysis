# 🎬 Netflix Content Analytics Dashboard (Power BI)

An interactive **Power BI** dashboard exploring Netflix’s catalog to uncover patterns in **content growth, ratings, audience age groups, and popularity**. This repo includes the raw dataset, a cleaned dataset, and the `.pbix` file to reproduce and extend the analysis.

> **Why this repo?** It shows end‑to‑end analytics: data cleaning ➜ semantic model & DAX ➜ storytelling visuals for quick, evidence‑based insights.

---
## ✨ What’s Inside
- **Global Catalog Overview**: Total titles, Movies vs Shows split, average IMDb score, audience age groups.
- **Ratings & Popularity**: Distribution of low/mid/high-rated titles, IMDb score vs votes (scatter), **Top 10 by votes**.
- **Growth & Trends**: Yearly releases, runtime trends, and age rating mix across decades.
- **Interactive Slicers**: Type (Movie/TV Show), Decade, Age Rating, Country, and more.

---

## 🗂️ Repo Structure
```
.
├─ Netflix Dashboard.pbix                 # Power BI report
├─ Netflix TV Shows and Movies.csv        # Raw dataset (as used)
├─ Netflix TV Shows and Movies_cleaned.csv# Cleaned dataset exported from Power BI / Power Query
├─ README.md

```
---

## 🧹 Data Cleaning (Power Query highlights)
- Trim/clean text columns; standardize **Type**, **Age Rating**, **Country** (split/explode lists if needed).
- Convert **Runtime** (“90 min”) → numeric minutes.
- Remove obvious duplicates and fix missing values where reasonable.
- Create **Decade** from `Release Year` (e.g., 2010s, 2020s).
- Exported a tidy snapshot as `Netflix TV Shows and Movies_cleaned.csv` for portability.

---

## 📐 Model & DAX (selected)
> Update table/column names if yours differ. These examples assume a table named **`Titles`** with columns like `Type`, `Release Year`, `IMDb Score`, `Votes`, `Runtime (min)`, `Age Rating`.

**Total Titles**
```DAX
Total Titles = COUNTROWS('Titles')
```

**Movies vs Shows**
```DAX
Movies = CALCULATE([Total Titles], 'Titles'[Type] = "Movie")
TV Shows = CALCULATE([Total Titles], 'Titles'[Type] = "TV Show")
```

**Average IMDb Score**
```DAX
Avg IMDb Score = AVERAGE('Titles'[IMDb Score])
```

**Avg IMDb Score (Last 5 Years)**
```DAX
Avg IMDb Score (Last 5 Years) =
VAR MaxYear = MAX('Titles'[Release Year])
RETURN
AVERAGEX(
    FILTER(
        ALL('Titles'),
        'Titles'[Release Year] >= MaxYear - 4 &&
        'Titles'[Release Year] <= MaxYear
    ),
    'Titles'[IMDb Score]
)
```

**% High‑Rated (IMDb ≥ 8.0)**
```DAX
Pct High Rated =
VAR High = CALCULATE(COUNTROWS('Titles'), 'Titles'[IMDb Score] >= 8)
RETURN DIVIDE(High, [Total Titles])
```

**Avg Runtime (by Type)**
```DAX
Avg Runtime (min) = AVERAGE('Titles'[Runtime (min)])
```

> Use these in **cards**, **clustered bars**, and a **scatter plot** (IMDb Score vs Votes, size by Runtime or use Title as detail).

---

## 📈 Dashboard Pages 
1. **Catalog Overview** — KPI tiles (Total Titles, Movies/Shows, Avg IMDb, % High‑Rated), donut for Type split, stacked bar by **Age Rating**, bar by **Decade**.
2. **Ratings & Popularity** — histogram of IMDb scores, scatter (Score vs Votes), **Top 10 by Votes** table.
3. **Growth & Runtime** — releases by year, **Avg Runtime** by Type & Decade.

---

## 👤 Author
**Aditya Kumar** — Data & BI enthusiast  
Suggestions & PRs welcome!
