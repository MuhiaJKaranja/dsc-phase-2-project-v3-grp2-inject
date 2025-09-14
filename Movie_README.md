# 🎬 Movie Studio Investment Analysis

This notebook explores movie performance data to help our company decide **what types of films to create**.  
We will use exploratory data analysis and statistical modeling to answer business questions about ROI.


> **Deliverables included (starter set):**
> - `README.md` — Notebook summary and guide
> - `presentation.pptx` (or `presentation.md`) — Non-technical presentation for Management
> - `student-checkpoint.ipynb` — analysis notebook developed by the project team
> - `data/` — expected location for raw inputs (e.g., `zippedData/...`), usually **gitignored**

---

## 1) Overview

**Business Problem.** The company is launching a **new movie studio** and needs evidence-based guidance on **what kinds of films to green-light**.

**Objective.** Use exploratory data analysis to identify patterns linked to **Return on Investment (ROI)** and produce **three concrete recommendations** for slate strategy.

**Current Scope.** We begin with:
- **The Numbers** (`tn.movie_budgets.csv.gz`): budgets and grosses
- **IMDB** (`im.db` → `movie_basics`): title, year, runtime, genres

The workflow is **modular** so teammates can add Rotten Tomatoes, TMDB, Box Office Mojo, etc.

---

## 2) Business Understanding

**Stakeholder.** Head of the new studio (green-lighting decisions).

**Key Questions.**
1. **Genres vs ROI** – Which genres yield the best returns?
2. **Budget vs ROI %** – Are bigger budgets more or less profitable?
3. **Runtime vs ROI** – Does movie length impact profitability?

**Decision Use.** Prioritize genre slate, design budget bands, and define runtime guardrails by genre.

---

## 3) Data Understanding & Preparation

**Sources.**
- **The Numbers** — production budgets, domestic/worldwide grosses, release dates.
- **IMDB** — movie metadata (titles, start_year, runtime_minutes, genres).

**Join Logic.** Normalize titles and match on **title + year** to merge The Numbers with IMDB.

**Target Metric.** `ROI = (worldwide_gross − production_budget) / production_budget` (requires budget > 0).

**Cleaning.**
- Cast currency strings to numeric.
- Drop missing/non-finite ROI.
- **Multi-genre policy (simple):** explode genres; each film contributes to every genre it’s labeled with. Results are **directional** because genres overlap.

---

## 4) Methods (aligned to syllabus)

- **Descriptive statistics:** counts, mean/median ROI, % profitable (ROI > 0).
- **t-based confidence intervals** (n ≥ 30; CLT justification).
- **Optional one-sample t-tests:** compare each genre’s mean to the overall mean.
- **Simple linear regression** (StatsModels): ROI ~ log10(budget), ROI ~ runtime.
- **Diagnostics:** QQ plot (normality) and residuals vs fitted (homoscedasticity).
- **Outlier awareness:** distributions plotted; optional winsorization/log transforms for sensitivity.

---

## 5) Analysis Summary (current iteration)

### 5.1 Genres vs ROI (Simple Multi-Genre)
- Per-genre **n, mean ROI, median ROI, % profitable**.
- Visuals: Top 10 by **Avg ROI**, Top 10 by **% Profitable**, **boxplots** (outliers hidden).  
- Interpretation: Favor genres above overall Avg ROI **and** with high % profitable; medians close to means suggest less outlier risk.

### 5.2 Budget vs ROI
- Regression **ROI ~ log10(budget)** with diagnostics; informs budget bands and slate mix.

### 5.3 Runtime vs ROI
- Regression **ROI ~ runtime_minutes** with diagnostics; checks for diminishing returns with very long runtimes.
- Visuals: Runtime vs Return on Investment with rehression line.
- Interpretation:  The graph shows that runtime has no meaningful effect on profitability.

> Teammates can extend with Rotten Tomatoes/TMDB (ratings, votes), cast/star power, franchise flags, marketing proxies.

---

## 6) Recommendations (draft; refine with added evidence)

1. **Slate focus:** Prioritize the top 2–3 genres that are above the overall Avg ROI **and** show high % profitable with medians close to means.
2. **Budget discipline:** Concentrate budgets in bands where ROI is resilient. Use a **tiered slate** (a few mid–high budget bets plus a steady pipeline of mid/low budgets).
3. **Runtime guardrails:** Avoid extreme runtimes unless the genre historically sustains them; target the runtime range with stable ROI in the regression.

**Next iteration:** Add confidence intervals to slides, perform sensitivity (winsorized/log ROI), and enrich with ratings and cast variables.

---

## 7) How to Run

**Environment:** Python 3.x; `pandas`, `numpy`, `matplotlib`, `scipy`, `statsmodels`, `sqlite3` (stdlib).

**Data layout (example):**
```
data/
└── zippedData/
    ├── tn.movie_budgets.csv.gz
    └── im.db
```

**Notebook:**
1. Open `student-checkpoint.ipynb`.
2. Run all cells (update any paths if needed).
3. Export charts to the `figures/` folder for the deck.

**.gitignore suggestions:** `/data/`, `*.zip`, `*.gz`, `*.db`, `*.DS_Store`

---

## 8) Repository Structure (suggested)

```
.
├── README.md
├── presentation.pptx            
├── student-checkpoint.ipynb
├── notebooks/                   
├── figures/                     
├── data/
│   └── zippedData/              # raw inputs (not tracked by git)
├── .gitignore
└── LICENSE (optional)
```

---

## 9) Team Collaboration Notes

- Add new analyses as **separate notebooks** (e.g., `notebooks/ratings_analysis.ipynb`).
- Export figures to `/figures` and reference them in `presentation.pptx`.
- Use **clear commit messages** (`feat:`, `fix:`, `viz:`, `doc:`) and keep commits small.

---

## 10) Limitations & Next Steps

**Limitations:** multi-genre overlap (directional results), ROI skewness (blockbusters), imperfect title-year joins.

**Next Steps:** add t-CIs to slides; winsorized/log ROI sensitivity; integrate ratings/votes/star power/franchise; consider multivariate/regularized regression when feature set grows.

---

## 11) Contact
Owner: _Your Name_ · _your.email@example.com_ · _LinkedIn URL_  
Collaborators: _Teammate A, Teammate B, …_
