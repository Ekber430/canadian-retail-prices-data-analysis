# Canadian Retail Prices Data Analysis

A data mining project exploring Canadian retail price trends, regional differences, province clustering, and time-series forecasting using Python.

[Analysis Notebook](Retail_Prices_Final_Project.ipynb) | [Project Report](Data_Mining_Final_Project_Report.pdf)

## Project Objectives

- Compare average prices of essential and non-essential products over time.
- Examine price distributions across encoded provinces.
- Test whether mean prices differ across provinces.
- Identify groups of provinces with similar quarterly price profiles.
- Forecast essential-product prices and evaluate prediction accuracy.

## Dataset

The dataset includes retail prices, product classifications, encoded geographic regions, and tax-related attributes. The saved visualizations cover 2017 to early 2025.

| Features | Description |
| --- | --- |
| Year, Month | Observation period |
| GEO | Encoded geographic region |
| Products, Product Category | Product name and category |
| VALUE | Original price per unit before tax |
| Essential | Essential or non-essential classification |
| Taxable, Total tax rate, Value after tax | Tax-related information |
| UOM, COORDINATE | Unit of measure and product identifier |

The analysis uses before-tax prices from `VALUE`. Geographic labels are encoded as Province 1, Province 2, and so on.

## Tools and Technologies

**Python · pandas · NumPy · Matplotlib · SciPy · scikit-learn · statsmodels · Jupyter Notebook**

A Power BI dashboard file is also included in the dataset directory.

## Methodology

### Data Preparation

- Converted year and month into datetime values.
- Cleaned geographic labels and converted prices to numeric values.
- Removed observations missing required date, province, or price values.
- Created monthly and quarterly features.
- Checked missing values and duplicate records.

The saved validation output shows **zero missing values and zero duplicate rows after preprocessing**.

### Exploratory Data Analysis

Created time-series charts, a province-level heatmap, boxplots, category histograms with density estimates, and a feature-correlation heatmap.

### Statistical Analysis

Applied **one-way ANOVA** to compare mean prices across provinces.

### Clustering

Constructed quarterly average-price profiles for each province and evaluated **K-Means with 2–7 clusters** using elbow and silhouette diagnostics. Fitted a final three-cluster model and used **PCA** for two-dimensional visualization.

### Forecasting

Fitted **ARIMA(1, 1, 1)** to monthly average essential-product prices. The last 12 monthly observations were reserved for testing, with performance measured using RMSE, MAE, and MAPE.

## Results

The figures and metrics below are taken from the notebook's saved outputs.

### Essential and Non-Essential Price Trends

Both categories show increasing average prices over the observed period, particularly around 2021–2023.

![Average price trends](figures/average-price-trends.png)

These values are averages across the included products, rather than the cost of a standardized shopping basket.

### Regional Price Differences

The heatmap summarizes monthly average prices across encoded provinces.

![Province price heatmap](figures/province-price-heatmap.png)

The boxplots compare provincial price distributions.

![Provincial price distributions](figures/province-price-distributions.png)

| Test | Result |
| --- | --- |
| ANOVA F-statistic | 6.31 |
| Printed p-value | 0.0000, rounded to four decimal places |

The ANOVA result provides evidence against equal mean prices across all groups under the test's assumptions. The p-value is rounded, not exactly zero. This test alone does not identify which province pairs differ.

### Province Clustering

The final K-Means model produced the following groups:

| Cluster | Provinces |
| --- | --- |
| 0 | Province 3, Province 4, Province 5 |
| 1 | Province 1, Province 7, Province 8, Province 9 |
| 2 | Province 2, Province 6, Province 10, Province 11 |

![Province clusters in PCA space](figures/province-clusters-pca.png)

Cluster 1 has lower average prices than the other two cluster centroids across the plotted quarters. All three show broadly rising price trajectories.

![Cluster centroids over time](figures/cluster-centroids.png)

The notebook selects **k = 3**, although the highest saved silhouette score occurs at **k = 2**. The three-cluster result is therefore an exploratory grouping rather than the silhouette-optimal solution.

### ARIMA Forecasting

| Model | Test Horizon | RMSE | MAE | MAPE |
| --- | --- | --- | --- | --- |
| ARIMA(1, 1, 1) | 12 months | 0.10 | 0.09 | 1.32% |

![ARIMA forecast](figures/arima-forecast.png)

The shaded area represents the model's forecast interval. RMSE and MAE use the target series' price units; MAPE is expressed as a percentage.

Although the original chart title says “CPI,” the forecast target is an **unweighted average of essential-product prices**, not an official Consumer Price Index.

## Repository Contents

| File or Folder | Description |
| --- | --- |
| `Retail_Prices_Final_Project.ipynb` | Analysis code and saved results |
| `Data_Mining_Final_Project_Report.pdf` | Project report |
| `dataset/` | CSV dataset, data dictionary, and Power BI dashboard |
| `figures/` | Figures exported from the notebook |

## How to Run

1. Clone the repository:

   ```bash
   git clone https://github.com/Ekber430/canadian-retail-prices-data-analysis.git
   cd canadian-retail-prices-data-analysis
   ```

2. Install the dependencies in your Python environment:

   ```bash
   python -m pip install pandas numpy matplotlib scipy scikit-learn statsmodels jupyter ipykernel
   ```

3. Open `Retail_Prices_Final_Project.ipynb` in Jupyter or VS Code. For VS Code, install the Python and Jupyter extensions and select your Python environment.

4. Update the data-loading cell to use:

   ```python
   file_path = 'dataset/Retail_Prices_of _Products.csv'
   ```

   The space between `of` and `_Products` is part of the filename. Run the notebook with the repository root as the working directory.

5. Run the cells from top to bottom.

The original dependency versions are not pinned. Results may vary across environments; the figures and metrics above describe the saved notebook run.

## Limitations and Future Improvements

- Average prices across different products and units do not constitute a basket-weighted inflation index.
- The ANOVA does not explicitly control for product mix or repeated observations over time.
- Clustering uses unscaled quarterly mean prices and fills missing combinations with zero, which can influence distances.
- Forecasting uses one holdout period and one ARIMA configuration. Baseline comparisons and rolling-window evaluation would strengthen the assessment.
- Tax information is present in the dataset, but the notebook does not estimate causal effects of tax policies.