# ES Migration | Population + Pharmacy | Brand Drug Use tile

## 1. Summary

Move the **Brand Drug Use** tile from MySQL to the ES service.

- Today, this project reads the data with MySQL queries.
- After the change, the data comes from ES service APIs.
- The user must see **the same thing as before**. Only the data source changes.

## 2. Where

| | Population page | Pharmacy page |
|---|---|---|
| URL | `/pop01/index` | `/phm01/index` |
| Tile name | "All Pharmacy: Brand Drug Use" | "Pharmacy: Brand Drug Use" |
| Tab 1: Spend chart | `POPSTK001` (`z5popstk001`) | `z5phmstk001` |
| Tab 2: PMPM chart | `z5popstk002` | `z5phmstk002` |
| Summary box (right side) | `POPSUM010` (`z5popsum010`) | `PHMSUM005` (`z5phmsum005`) |

PMPM = cost **p**er **m**ember **p**er **m**onth.

## 3. Expected behavior (must not change)

### 3.1 Chart (left side)

- It has 2 tabs: **Spend** and **PMPM**.
- It shows 2 colored areas: **Brand Drug Spend** and **Generic Drug Spend**.
- It shows **4 points** on the time line:
  - 3 past years (oldest first).
  - 1 **projected** year at the end. This is a guess for next year.
- The Y axis says **"Spend"** on the Spend tab and **"PMPM"** on the PMPM tab.
- Click a point to see a small pop-up with:
  - Year and cost.
  - Member count.
- A click does **not** open a member list.

### 3.2 Summary box (right side)

| Label | What it means |
|---|---|
| Brand Drug Spend | Brand drug spend in the latest year |
| % Change (Spend) | Change in brand spend from last year to the latest year |
| Generic Drug Spend | Generic drug spend in the latest year |
| Savings Available | Money we could save by moving from brand to generic |

- % Change goes **up**: red text, "Increase".
- % Change goes **down**: green text, "Decrease".
- The **Take Action** button opens `/act01/index`.

### 3.3 Other things on the tile

- The **help (?) icon** works.
- **Add to story:**
  - Population: it shows only when **no** cohort filters are applied.
  - Pharmacy: it always shows.
- **Custom period:** if the user picks custom dates, the tile uses those dates.

