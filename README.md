# Who Actually Pays the Airline Competition Premium?

### A data-mining investigation of US airfares, 1993–2024
#### Author: Ankitha Prasad

> **Start here:** [`main_notebook.ipynb`](main_notebook.ipynb)

> 🎥 **Project video:** [_\[Airline Pricing Project Video\]_](https://www.youtube.com/watch?v=0VFnAAqlGkM)

---

## Overview

Do we actually pay more when one airline dominates a route or an airport?

This project started from a personal experience — flights out of College Station (a small, single-carrier-dominated airport) seemed noticeably more expensive than flights out of nearby cities like Houston or Austin. The 'obvious' answer is yes, less competition means higher prices. But this project tests that intuition against three decades of US domestic airline data and finds a much more nuanced story.

Using the U.S. DOT airline market data from 1993–2024, the analysis works through baseline fare modeling, market-concentration regressions, carrier-type stratification, an airport-level network ('fortress hub') analysis, and a CPI-adjusted time trend. The headline finding: **simple route-level monopoly does not raise fares — but airport-level legacy-carrier dominance does, and that premium has grown noticeably since the post-2010 wave of airline mergers.**


## Research Questions

1. **The starting question:** Do monopoly routes — where one airline holds ≥90% market share — have higher fares than competitive routes, after controlling for distance, demand, year, and seasonality?
2. **The diagnostic question:** If route-level monopoly does not predict higher fares, is that because the "monopoly" label is mixing legacy carriers with low-cost carriers (ULCCs)?
3. **The reframed question:** Does market power show up at the **airport** level instead — at legacy "fortress hubs" — rather than at the individual route level?
4. **The time question:** Has the legacy-carrier monopoly premium changed over time, especially around the 2008–2013 wave of major airline mergers?

## Data

- **Source:** [Kaggle — US Airline Flight Routes and Fares 1993–2024](https://www.kaggle.com/datasets/bhavikjikadara/us-airline-flight-routes-and-fares-1993-2024)
- **File in repo:** [`data/airline_data.csv`](data/airline_data.csv) (~63 MB)
- **Coverage:** ~245k route-quarter observations across ~212 airports and ~1,266 undirected routes spanning 1993 through 2024.
- **Key fields used:** `year`, `quarter`, `city1`/`city2`, `airport_1`/`airport_2`, `nsmiles` (distance), `passengers`, `fare`, `carrier_lg` (largest carrier), `large_ms` (largest market share), `carrier_low`, `lf_ms`.

### Preprocessing (done in [`main_notebook.ipynb`](main_notebook.ipynb), Phase 1)
- Lowercase all column names.
- Drop self-routes (origin == destination), non-positive fares, and rows with invalid market shares.
- Drop rows missing fare, distance, passengers, year, quarter, carrier, market share, airport, or city.
- Build an undirected route key so A→B and B→A are treated as the same route.
- Cast `year` and `quarter` to integers; require ≥1 passenger per row.
- **Deliberately keep high-fare outliers** — the project is specifically about whether dominance produces unusually high fares, so trimming them upfront would hide the signal.
- Apply CPI adjustment to 2024 dollars for the time-trend phase.


## How to Reproduce

This project was built and run in **Google Colab**. The recommended path is:

1. Clone this repo or open the notebooks directly in Colab.
2. Either upload [`data/airline_data.csv`](data/airline_data.csv) to your Colab environment, or download it from the [Kaggle source](https://www.kaggle.com/datasets/bhavikjikadara/us-airline-flight-routes-and-fares-1993-2024) and adjust the data-loading path in Phase 0 of the main notebook.
3. Install pinned dependencies with `pip install -r requirements.txt`.
4. Run the notebooks in this order:
   1. [`checkpoints/checkpoint_1.ipynb`](checkpoints/checkpoint_1.ipynb) — early exploration and cleaning decisions.
   2. [`checkpoints/checkpoint_2.ipynb`](checkpoints/checkpoint_2.ipynb) — mid-semester modeling and graph experiments.
   3. [`main_notebook.ipynb`](main_notebook.ipynb) — **the curated final story**, runnable top-to-bottom on its own.

The main notebook is fully self-contained — the checkpoint notebooks are included to show the progression of work over the semester, but you do not need to run them to reproduce the final results.

## Key Dependencies

Built in Google Colab with **Python 3.12.13**. Main packages:

- Python 3.x
- pandas
- numpy
- matplotlib
- seaborn
- statsmodels
- scikit-learn
- networkx

The complete pinned environment lives in [`requirements.txt`](requirements.txt).

## Repository Structure

```
airline-pricing-project/
├── README.md                            ← you are here
├── main_notebook.ipynb                  ← the curated final deliverable
├── requirements.txt                     ← pip freeze from the Colab session
├── .gitignore
├── checkpoints/
│   ├── checkpoint_1.ipynb               ← Project Checkpoint 1
│   └── checkpoint_2.ipynb               ← Project Checkpoint 2
└── data/
    └── airline_data.csv                 ← raw Kaggle dataset (1993–2024)
└── assets/
    └── Airline_Premium_Pitch_Deck.pptx  ← Final Presentation
```

---

## Results Summary

The project answers its starting question in three layers:

- Route-level monopoly does NOT increase fares
- Airport dominance (fortress hubs) DOES increase fares
- Example: DFW vs MDW → ~$57 higher fares
- Effect becomes stronger after ~2015

For the full analysis — including baseline modeling, robustness checks, the airport graph construction, and what didn't work — see [`main_notebook.ipynb`](main_notebook.ipynb).
