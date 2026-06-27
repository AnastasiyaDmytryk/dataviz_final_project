# Who Were You, Miss Girl? — An Analysis of Pinterest Users Aged 18–24

**Author:** Anastasiya Dmytryk
**Course:** Data Visualization and Reproducible Research — Final Project

## Motivation

Pinterest is a platform millions of women turn to for inspiration, and it's also my dream company to work for. Pinterest publishes a lot of its audience data publicly, which made it a great candidate for a deeper dive. Rather than analyze the platform as a whole, this project zooms in on a single demographic: women aged 18–24, and tries to answer a more specific, more human question: based purely on what this group searched for and saved throughout 2025, what does a "typical" Pinterest girl in this age range actually look like? Where does she live, what's she into each season, and how does her year rise and fall with school schedules, holidays, and trends?

This project also doubles as a personal exercise: I'm in this exact age range, so I treated myself as something of a built-in subject-matter expert when interpreting what the data was telling me.

## Data

Two datasets power this analysis:

1. **A custom trends dataset** (`data/normalized_enriched_trends.csv`) that I collected and enriched with an LLM for an earlier course project in Data Wrangling. It tracks weekly search volume across four base keywords: nails, fashion, beauty, and makeup. After it is broken out further by specific trend, age group, gender, and a set of LLM-assigned descriptive tags (theme, occasion, category) that needed cleaning and re-deriving (e.g., seasons were recalculated directly from the date rather than trusting the LLM's own season labels).
2. **Pinterest's own publicly available audience insight statistics** — specifically an age-distribution breakdown and a U.S. metro-area audience breakdown, both originally presented by Pinterest as fairly plain, static bar charts. I used these as a starting point and redesigned both into more visually engaging, information-dense formats (see Redesign section below).

## What's in this project

- **Age overview** — a redesigned, image-based version of Pinterest's age-distribution chart, using a colorblind-safe (Okabe-Ito) palette.
- **Geographic distribution** — Pinterest's original U.S. audience bar chart redesigned as an actual map, with dot size showing estimated audience share by metro area. This is the project's **before/after chart redesign**: the original bar chart lists city names and percentages but can't show *where* those cities actually are relative to each other, which obscures real geographic patterns that the map makes immediately visible.
- **Monthly interest by category** — an interactive 4-panel chart (hover for exact values per month) comparing normalized search interest in nails, fashion, beauty, and makeup across 2025, built with a colorblind-safe Dark2 palette.
- **Monthly top-trend timeline** — an interactive timeline highlighting the single most-searched trend each month, letting the reader scrub across the year and see exactly which trend was the most popular each month and why.
- **Seasonal theme flow** — an interactive Sankey diagram  tracing how the top 5 trending themes shift across spring, summer, fall, and winter, using a colorblind-safe viridis palette.
- **Interactive persona widget** — a clickable character (built in HTML/CSS/JS, embedded via an R chunk) where clicking the hair, face, outfit, or nails reveals the actual top-performing trend for that category among 18–24 year olds, calculated directly from the dataset.
- **LLM Enhancement** - the subset of 18-24 age group os give to an llm and the llm is asked to create a mood board and a pre season visualization of an average character. 
- **Conclusion** — a combination of everything above into a single composite "persona," plus a reflection on what the project shows about how trend data can tell a story when given enough context (season, geography, category) rather than read as raw numbers alone.

## My favorite visualization

The **seasonal theme flow Sankey diagram** is my favorite. It's the chart that made the most disconnected and confusing pieces of the analysis cfall into a pretty graph that is esy to follow. Uou can visually trace, for example, how "edgy/dark" aesthetics dominate both fall and winter, while nail-related themes spike specifically around summer vacation and the winter holidays. It's also the most genuinely interactive piece in the project on hover it reveals exact average interest scores for each season-theme pairing, which static charts simply can't offer.

## Accessibility notes

- All color-coded charts use verified colorblind-safe palettes: Okabe-Ito (age chart), ColorBrewer Dark2 (keyword charts), and viridis (Sankey themes).
- Every figure includes alt text via the `fig.alt` chunk option.
- Where color is the primary visual encoding (e.g., the Sankey), an explicit text legend or on-chart labeling is also provided so color is never the only way to identify a category.

## Limitations & future directions

- The underlying trends dataset reflects a self-selected set of search categories (nails, fashion, beauty, makeup) chosen by me, so it captures one specific lens on this demographic rather than the full range of what 18–24 year old Pinterest users engage with.
- LLM-assigned enrichment tags (theme, occasion, category) were inconsistent and required substantial cleanup; a few categories were collapsed using pattern-matching rules that, while reasonable, involved some judgment calls and could be revisited with a more rigorous taxonomy.
- Geographic figures are estimates derived by combining Pinterest's published audience breakdowns with general population data, not direct platform-reported numbers for this specific age/gender/metro combination.
- Future work could expand the dataset beyond four categories, incorporate actual multi-year data to check whether the seasonal patterns found here hold up year over year, or replace some of the manual LLM-tag cleanup with a more systematic NLP classification pipeline.

## Repo structure

```
project_03/
├── finalproject.Rmd
├── finalproject.html
├── finalproject.md
├── images/
├── outputs/              # self-contained interactive HTML exports
└── data/
    └── normalized_enriched_trends.csv
```
