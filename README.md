# 🚗 Car Dataset Analysis

This project cleans the *Car Features & MSRP* dataset (11,914 listings), adds engineered features, and explores it to answer four questions about price, horsepower, drivetrain and fuel economy.

## Project structure

```
Car Dataset Analysis Project/
├── data/
│   ├── raw/cars.csv                 # original dataset (unchanged)
│   └── processed/cars_cleaned.csv   # cleaned + engineered dataset (10,603 rows × 19 cols)
├── notebooks/
│   └── car_dataset_analysis.ipynb   # full analysis in German (runs locally or on Google Colab)
├── reports/
│   └── figures/                     # every chart, exported as PNG
├── presentation/
│   └── Auto-Datensatz Analyse.pptx  # project presentation (German, 13 slides)
├── CHECKLIST.md                     # status of every task in the project brief
├── requirements.txt
└── README.md
```

## Presentation

**[Auto-Datensatz Analyse.pptx](presentation/Auto-Datensatz%20Analyse.pptx)**: a 13-slide PowerPoint deck in German that presents the project, with speaker notes on every slide.

| Part | Slides |
|---|---|
| Introduction | Title · the four questions |
| Data | From raw data to analysis base · data cleaning · placeholder prices before 2001 · feature engineering |
| Results | Price by vehicle size · horsepower vs. price · price by drivetrain · MPG by transmission · correlation matrix |
| Conclusion | Four key takeaways · thank-you and Q&A |

## How to run

**Locally**
```bash
pip install -r requirements.txt
jupyter notebook notebooks/car_dataset_analysis.ipynb
```

**Google Colab:** upload the notebook and choose *Runtime → Run all*. If `data/raw/cars.csv` isn't present, the notebook downloads the dataset from its public URL.

## Pipeline

| Step | What was done |
|---|---|
| **Cleaning** | Dropped 715 duplicate rows. Filled missing values with group-wise medians or modes (Make + Model, then broader groups). Set cylinders to 0 for electric cars and Mazda's rotary engines. Filled missing `Market Category` with `"none"`. Corrected a 354 highway-MPG typo to 35. No rows were dropped for missing values. |
| **Types** | Integer columns (`Year`, cylinders, doors, MPG, MSRP, popularity) are `int64`. Text columns are `category`, and `Vehicle Size` is an ordered category. |
| **Filter** | Model year ≥ 1995. |
| **Strings** | `Vehicle Style` and `Market Category` converted to lowercase. |
| **Features** | `Total MPG` = mean of city and highway MPG. `Price per HP` = MSRP / Engine HP. `MSRP Placeholder` flags cars from 2000 or earlier whose MSRP isn't a real list price. |
| **EDA** | Descriptive statistics, group averages, 6 required charts plus one extra, Kruskal–Wallis and Mann–Whitney tests, and Pearson and Spearman correlations. |

## Key findings

- **Price data quality:** for model years up to 2000, MSRP is a placeholder (median ≈ $2.3k). Price analyses therefore use only the 9,831 cars from 2001 onward.
- **Vehicle size:** median prices rise from $25.7k (compact) to $34.7k (midsize) to $42.6k (large).
- **Horsepower and price:** horsepower is the strongest price driver (Spearman ρ = 0.85). Price grows roughly exponentially with horsepower.
- **Drivetrain and price:** prices differ significantly by drivetrain (p < 0.001). Front-wheel drive is the cheapest, and all-wheel and rear-wheel drive are not significantly different from each other.
- **Transmission and MPG:** among combustion and hybrid cars the differences are small. Automated-manual gearboxes do best (20.4 city / 28.3 highway MPG), then manual, then automatic. Electric cars are reported separately because their ratings are in MPGe.
- **Popularity:** the popularity score is assigned per brand and is unrelated to price, horsepower or MPG.

The full write-up is in the **Fazit** (conclusion) section at the end of the notebook.
