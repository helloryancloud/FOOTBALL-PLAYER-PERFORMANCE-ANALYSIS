# ⚽ FIFA World Cup 2026 – Player Performance Analysis (Power BI)

An interactive two-page Power BI dashboard that analyses player performance, physical attributes, discipline, and tactical intensity in the FIFA World Cup 2026 player performance dataset. All metrics are built with **DAX**.

> **Note:** The dataset is **synthetically generated**. Findings describe patterns in generated data, not real football results.

---

## 📌 Project Overview

The goal of this project is to turn raw match-level player statistics into clear, decision-ready insights. The analysis answers questions about finishing efficiency, chance creation, goalkeeping, physical build, discipline, tactical workload, and age.

**Tools used:** Power BI Desktop · DAX · Excel/CSV

---

## 📂 Dataset

- **Source:** Kaggle – *FIFA World Cup 2026 Player Performance Dataset*
- **Size:** 54,600 rows × 75 columns
- **Grain:** one row per player per match
- **Coverage:** 1,248 players, 1,050 matches

**Column groups include:**
- Player profile: `player_id`, `player_name`, `age`, `nationality`, `team`, `position`
- Physical attributes: `height_cm`, `weight_kg`, `preferred_foot`
- Offensive: `goals`, `assists`, `shots`, `shots_on_target`, `key_passes`
- Defensive: `tackles`, `interceptions`, `clearances`, `saves`, `clean_sheet`
- Discipline: `yellow_cards`, `red_cards`
- Advanced: `expected_goals_xg`, `expected_assists_xa`, `distance_covered_km`, `sprint_distance_km`, `player_rating`

### ⚠️ Data note
Columns such as `total_*_tournament` repeat the same cumulative value on every row of a player. Summing them would multiply the totals, so the analysis uses only the match-level columns with `SUM` or `AVERAGE`.

---

## ❓ Business Questions Answered

### 1. Player Performance and Efficiency
- Who are the most clinical forwards? (goals vs xG)
- Which playmakers create the highest quality chances?
- Who is the most dominant goalkeeper?

### 2. Physicality and Tactics
- Does physical build (height and weight) affect defensive output?
- Does preferred foot relate to position?

### 3. Squad Analytics
- Which team is the most disciplined?
- Which team has the most physically demanding style?

### 4. Age Dynamics
- How does age relate to player rating?

---

## 🧮 DAX Measures

**Base measures**
```DAX
Total Goals = SUM(Players[goals])
Total xG = SUM(Players[expected_goals_xg])
Total Assists = SUM(Players[assists])
Total Key Passes = SUM(Players[key_passes])
Total Shots = SUM(Players[shots])
Total Shots On Target = SUM(Players[shots_on_target])
Avg Player Rating = AVERAGE(Players[player_rating])
```

**Efficiency**
```DAX
Goals Minus xG = [Total Goals] - [Total xG]

Shot Accuracy % =
DIVIDE([Total Shots On Target], [Total Shots], 0)

Chance Creation Score = [Total Assists] + [Total Key Passes]
```

**Goalkeepers**
```DAX
GK Save % =
DIVIDE(
    [Total Saves],
    [Total Saves] + [Total Goals Conceded],
    0
)
```

**Physicality and defence**
```DAX
Defensive Output =
SUM(Players[clearances]) + SUM(Players[interceptions])

Height Band =
SWITCH(
    TRUE(),
    Players[height_cm] < 170, "Under 170",
    Players[height_cm] < 180, "170-179",
    Players[height_cm] < 190, "180-189",
    "190+"
)
```

**Discipline and workload**
```DAX
Total Cards = SUM(Players[yellow_cards]) + SUM(Players[red_cards])
Avg Distance Covered = AVERAGE(Players[distance_covered_km])
Avg Sprint Distance = AVERAGE(Players[sprint_distance_km])
```

**Age**
```DAX
Age Group =
SWITCH(
    TRUE(),
    Players[age] <= 20, "20 & under",
    Players[age] <= 24, "21-24",
    Players[age] <= 28, "25-28",
    Players[age] <= 32, "29-32",
    "33+"
)
```

> Ratio measures use `DIVIDE()` to avoid divide-by-zero errors, and totals are divided before ratios are taken so players with few shots don't skew accuracy.

---

## 📊 Dashboard

### Page 1 – Player Performance and Age
| Visual | Purpose |
|---|---|
| KPI cards | Total Goals, Total Assists, Shot Accuracy %, Avg Player Rating |
| Scatter chart (xG vs Goals, Forwards) | Finds clinical finishers |
| Clustered bar (Assists + Key Passes, Top 10) | Ranks playmakers |
| Goalkeeper table | Saves, clean sheets, save %, rating |
| Column chart (Age Group vs Rating) | Shows how rating changes with age |

### Page 2 – Physicality, Tactics and Squad Analytics
| Visual | Purpose |
|---|---|
| KPI cards | Yellow Cards, Red Cards, Avg Distance, Avg Sprint Distance |
| Decomposition tree | Breaks down defensive output by position, height band, foot |
| 100% stacked column | Preferred foot by position |
| Stacked bar | Yellow and red cards by team |
| Scatter chart | Distance vs sprint distance by team |

**Slicers:** `team` and `position`, synced across both pages.

---

## 🔍 Key Findings

**Player performance**
- **Most clinical forward:** Memphis Zerrouki (Netherlands) scored 24 goals from just 3.22 xG, a difference of about +20.8.
- **Top playmaker:** Yassine El Yamiq (Morocco) recorded 13 assists and 75 key passes. John Armstrong (Scotland) had the most key passes (76).
- **Goalkeepers:** Nicolo Immobile (Italy) made the most saves (87). Hassan Abdulla (Qatar) and Joel Borges (Costa Rica) tied for the most clean sheets (4 each).

**Physicality and tactics**
- Height and weight have almost no effect on defensive output (correlation about 0.01–0.02). Position matters far more: defenders average about 1.74 clearances per match versus about 0.12 for forwards.
- Right foot is the majority in every position, at roughly 70–77%.

**Squad analytics**
- **Most disciplined:** Austria (77 cards). **Least disciplined:** Qatar (168 cards).
- Team differences in distance covered are tiny (about 3.99–4.07 km per match), with Panama highest at 4.07 km.

**Age**
- Average rating declines steadily with age: roughly 3.78 for under-20s down to about 3.26 for ages 33+.

---





Dataset: *FIFA World Cup 2026 Player Performance Dataset* on Kaggle.
