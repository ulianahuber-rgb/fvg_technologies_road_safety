# FVG Technologies | Road Safety Market Expansion in Italy

**Data Analytics Capstone Project · ISTAT 2020–2024 · Python · Machine Learning · Power BI**

> **Business question:** Which Italian municipalities should FVG Technologies assess first for expansion, and why?

## Company background and business objective

**FVG Technologies** is a **fictional Italian company** specialising in road safety technology, traffic monitoring and data analytics. In this case study, the company is based in **Udine, Friuli-Venezia Giulia**, and already operates in the provinces of **Udine, Trieste and Pordenone**. Its expansion plans focus **exclusively on Italy**.

The company wants to identify municipalities where it could propose road-safety solutions, work with local authorities and explore future public tenders. As a **Data Analyst**, my role is to turn official road-accident data into evidence that helps the business decide **which municipalities to investigate first**.

This is a **market-screening exercise**, not a list of confirmed contracts, available tenders or approved investments.

## Project overview

| Project measure | Result |
|---|---:|
| Period analysed | 2020–2024 |
| Recorded road accidents | **775,951** |
| People injured | **1,045,938** |
| Fatalities recorded | **14,498** |
| Cleaned municipality–year observations | **39,507** |
| Municipality-level observations used for clustering | **7,919** |

The analysis combines **total accident volumes**, **accidents per 100,000 residents** and **average annual accidents per km²** to identify different patterns across Italian municipalities.

## Data sources and preparation

- **[ISTAT road-traffic data](https://esploradati.istat.it/SDMXWS/rest/data/41_983):** accident, injury and fatality counts obtained through the ISTAT SDMX API.
- **[ISTAT SITUAS](https://situas.istat.it/):** population, geographical area and municipality information.

The sources were cleaned and merged at **municipality–year level**. The data preparation checked identifiers, duplicates, missing values and year coverage, resulting in **39,507 rows and 13 columns**.

**Data-quality note:** For 2024, **1,557** municipality records without matching traffic observations were treated as having zero accidents, injuries and fatalities. This is a processing assumption; missing records do not independently prove that no accidents occurred.

## Analysis workflow

### 1. Data preparation — `01_data_preparation.ipynb`

Retrieved and combined ISTAT and SITUAS data, validated the dataset and calculated relevant road-safety indicators.

### 2. Exploratory data analysis — `02_analysis.ipynb`

Explored trends over time, accident totals, rates adjusted for population, accident density, correlations, mortality indicators and outliers.

**Finding:** Large cities often have the highest numbers of accidents, but rankings change when accidents are compared with population or land area. These measures provide complementary insights for FVG Technologies.

### 3. Municipality clustering — `03_clustering_modeling.ipynb`

Compared K-Means clustering models using the **Silhouette Score**, **Calinski–Harabasz Index** and **elbow method**. Features were log-transformed and scaled before fitting the models.

The selected **K-Means model (k = 3)** uses accidents per 100,000 residents and accidents per km². The clusters were profiled using the original municipality-level indicators.

| Cluster profile | Municipalities |
|---|---:|
| Low Accident Activity | **6,225** |
| Minimal / No Accident Activity | **489** |
| Higher Accident Activity and Density | **1,205** |

The higher-activity cluster helps the company focus its research on areas with elevated accident indicators; it is **not a definitive risk ranking**.

**Why are there 995 selected municipalities in Power BI?** The clustering model identified **1,205** municipalities in the higher-activity group. A separate **DAX measure** applies an **average population threshold of 5,000 residents**, leaving **995** municipalities for further assessment. The other **210** are excluded only from this selection. Python confirmed the same result, and **all 7,919 observations remain in the exported dataset**.

### 4. Regression and prediction — `04_regression_modeling.ipynb`

Tested simple linear regression, multiple regression and a **persistence baseline** that uses the previous year's accident count as the next year's prediction.

Models were trained on **2021–2023** and evaluated on **2024**, using Mean Absolute Error (**MAE**; lower is better).

| Prediction method | 2024 test MAE |
|---|---:|
| **Persistence baseline** | **3.67** |
| Simple linear regression | 4.03 |
| Multiple linear regression | 4.19 |

**Finding:** The simple persistence baseline was the most accurate method overall on the 2024 test data. A statistically strong regression relationship does not always lead to better predictions.

## Business recommendations for FVG Technologies

Based on **average annual accident volumes**, the following municipalities are candidates for **further commercial and road-safety assessment**:

| Municipality | Accidents / year | Per 100k residents | Per km² | Potential area to investigate |
|---|---:|---:|---:|---|
| **Roma** | 11,856.0 | 430.4 | 9.20 | Large-scale traffic monitoring |
| **Milano** | 7,131.8 | 522.8 | 39.23 | High-density accident monitoring |
| **Genova** | 3,575.0 | 634.9 | 14.86 | Road-safety analytics and local risk assessment |
| **Torino** | 2,740.4 | 321.7 | 21.07 | Junction and traffic-flow analysis |
| **Napoli** | 2,243.8 | 244.8 | 18.83 | Traffic management and safety assessment |

These five cities are **ranked by accident volume**, not by expected business returns or confirmed local demand. **Roma** has the highest accident volume, **Milano** the highest accident density, and **Genova** the highest population-adjusted rate **among these five**.

**Suggested next steps:** review local accident severity and trends; investigate municipal needs, budgets and public-tender opportunities; then evaluate a suitable pilot project before deciding where to expand. The proposed solutions are possibilities, **not evidence of existing contracts or tenders**.

## Power BI dashboard

The interactive report includes **four pages**:

1. **Road Safety Overview:** national totals, accident trends, clustering and municipality comparisons.
2. **Municipality Deep Dive:** profiles, rates, cluster membership and recent local trends.
3. **Prediction Performance:** regression forecasts versus the persistence baseline for 2024.
4. **Investment Recommendations:** evidence supporting further assessment of potential target municipalities; this page can be presented as FVG Technologies' expansion recommendations.

The report retains the complete clustering dataset. The **995-municipality** selection is calculated separately using a DAX measure.

## Tools and technologies

**Python** (Pandas, NumPy, Matplotlib, Seaborn, scikit-learn, Statsmodels, Requests) · **Jupyter / VS Code** · **Power BI** · **DAX** · **GitHub**

## Repository structure

```text
fvg-technologies-road-safety/
├── README.md
├── 01_data_preparation.ipynb
├── 02_analysis.ipynb
├── 03_clustering_modeling.ipynb
├── 04_regression_modeling.ipynb
├── data/
│   ├── raw/
│   ├── cleaned/
│   └── powerbi/
└── powerbi/                  # Power BI report, if included
```

*This is the proposed layout. Notebook filenames should be standardised before publication; include datasets only when size and redistribution rules permit.*

## How to reproduce

1. Clone the repository and open the project root in Jupyter or VS Code.
2. Create a Python environment and install dependencies:
   ```bash
   python -m pip install pandas numpy matplotlib seaborn scikit-learn statsmodels requests jupyter
   ```
3. Make the required source files available, or retrieve them from the ISTAT sources above.
4. Run notebooks **01 → 02 → 03 → 04**, using the project root as the working directory for relative `data/...` paths.
5. Load the exported CSV files into Power BI to inspect the results.

*The notebook files include saved outputs. Full end-to-end reproducibility should be verified before publishing.*

## Limitations

- Accident rates per **100,000 residents** are not individual risk measures; commuting and tourism can affect their interpretation.
- The **5,000-resident threshold** is an exploratory selection criterion, not an official safety standard.
- The assumption about missing 2024 traffic records can affect trend estimates and model results.
- Forecast performance was tested on one holdout year (**2024**) and may differ in future periods.
- Local road conditions, traffic volumes, accident severity, costs and public-tender availability were **not fully assessed**. They must be investigated before commercial or investment decisions.

## Conclusion

This project demonstrates how **data preparation, EDA, clustering, regression and Power BI** can help **FVG Technologies** screen Italian municipalities for potential expansion. The analysis identifies candidate markets and explains why they merit attention, while leaving final decisions to further local, technical and commercial assessment.

*FVG Technologies and its operating history are fictional elements of this academic business scenario. All reported accident statistics are derived from the ISTAT/SITUAS project data and notebook results.*
