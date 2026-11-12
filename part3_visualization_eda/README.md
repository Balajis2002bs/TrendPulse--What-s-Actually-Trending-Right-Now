#  Trending Videos – Exploratory Data Analysis & Visualization

## Overview

This notebook performs exploratory data analysis (EDA) on YouTube trending-video records from **the United States (US), India (IN), and Great Britain (GB)**. It combines the country datasets, cleans selected missing and duplicate values, enriches the records with category names, and explores video performance and trending behaviour.

The analysis uses static and interactive visualizations, descriptive statistics, correlation, outlier detection, funnel-style conversion rates, trending retention, and cohort-based video lifespan analysis.

## Objectives

- Combine trending-video data from US, IN, and GB.
- Inspect data types, missing values, duplicate records, and video records marked as removed or erroneous.
- Compare video activity and engagement across countries and categories.
- Examine the distributions and outliers of views and likes.
- Explore relationships among numerical features.
- Build a funnel based on video views and comment counts.
- Measure how long videos remain on the trending list.
- Compare trending lifespan across weekly cohorts.
- Summarize findings as content-strategy observations.

## Project structure

The notebook expects the input CSV files to be available in a `data/` directory alongside the notebook:

```text
project/
├── part3_visualization_eda.ipynb
└── data/
    ├── USvideos.csv
    ├── INvideos.csv
    ├── GBvideos.csv
    └── categories.csv
```

The notebook reads these exact relative paths:

- `data/USvideos.csv`
- `data/INvideos.csv`
- `data/GBvideos.csv`
- `data/categories.csv`

The CSV files are required to run the notebook. They are not created by the notebook.

## Requirements

Python 3.9+ is recommended. Install the libraries used in the notebook:

```bash
pip install pandas numpy matplotlib seaborn plotly jupyter
```

## How to run

1. Clone or download this project and place the four CSV files in the `data/` folder shown above.
2. Open a terminal in the project directory.
3. Install the required packages.
4. Start Jupyter:

   ```bash
   jupyter notebook
   ```

5. Open `part3_visualization_eda.ipynb`.
6. Run the cells from top to bottom.

The notebook uses relative file paths, so run it with the project directory as the working directory.

## Workflow and analysis

### 1. Data loading and preparation

- Load the US, India, and GB trending-video CSV files and the category lookup CSV.
- Add a `country` column to identify the source country for each dataset.
- Concatenate the three country datasets into `combined_df`.
- Inspect the combined dataset's shape, data types, and missing values.
- Fill missing numeric values with `0` and missing descriptions with `"Unknown"`.
- Calculate missing-value percentages.
- Identify and remove duplicate rows.
- Merge category information using `category_id` with an inner join.
- Inspect records where `video_error_or_removed` is `True`.
- Convert `trending_date` and `publish_time` into datetime values.

### 2. Static visualizations with Matplotlib and Seaborn

The notebook creates a multi-panel EDA dashboard containing:

- Video record count by country.
- Distribution of log-transformed views.
- Number of videos trending by date.
- Views-versus-likes scatter plot.
- Views by category box plot.
- A text summary with total records, the country with the most records, average views, and the highest category median-view value.

### 3. Interactive visualizations with Plotly

- **Views vs. likes:** an interactive scatter plot, coloured by category, with video titles available on hover.
- **Average views by category and country:** an interactive bar chart comparing average views for each category across US, IN, and GB.

### 4. Distributions and outliers

- Calculate mean, median, and mode for views and likes.
- Calculate skewness to assess the shape of the distributions.
- Apply the interquartile range (IQR) rule to identify potential outliers in views and likes.
- Retain outliers for the subsequent analysis because high view and like counts are meaningful when studying trending-video performance.

### 5. Correlation analysis

- Calculate correlations between the numeric columns selected during data preparation.
- Display the correlation matrix as a Seaborn heatmap.

### 6. Funnel analysis

The notebook calculates four video counts:

1. **Stage 1:** unique videos that appeared in the trending data.
2. **Stage 2:** videos whose maximum recorded views are above the median of per-video maximum views.
3. **Stage 3:** videos whose maximum recorded views are above the 90th percentile of per-video maximum views.
4. **Stage 4:** videos whose maximum recorded comment count is at least 1,000.

It then calculates percentage conversion rates between consecutive stages.

### 7. Retention-style analysis

- Assign each video's observed trending dates a dense day index.
- Calculate the percentage of videos still present on observed trending days 2, 3, 5, and 7.
- Plot retention curves grouped by category and by country.

This is a **trending-list retention measure** based on the dates available in the dataset; it is not a measure of user or subscriber retention.

### 8. Cohort and trending-lifespan analysis

- Find each video's first observed trending date.
- Calculate each video's observed trending lifespan from its first to last trending date, inclusive.
- Group videos by the week of their first observed trending date.
- Calculate the average lifespan for each weekly cohort.

### 9. Content strategy briefing

The final notebook section summarizes its observations about:

- Category-level views, likes, and comments.
- How quickly videos appear on the trending list and how long they remain.
- Differences in trending retention between US, IN, and GB.

## Main Python libraries

| Library | Purpose |
|---|---|
| `pandas` | Loading, cleaning, combining, grouping, and analysing tabular data |
| `numpy` | Numerical operations and logarithmic transformation |
| `matplotlib` | Static charts and plots |
| `seaborn` | Statistical visualizations, including box plots and correlation heatmaps |
| `plotly.express` | Interactive scatter and bar charts |

## Important implementation notes

- The notebook expects `trending_date` values in the `%y.%d.%m` format when parsing that column.
- The category merge is an **inner join**. Rows without a matching `category_id` in `categories.csv` will therefore not remain in the merged dataset.
- The outlier section currently calculates `likes_iqr` using the views quartiles (`views_Q3 - views_Q1`) rather than the likes quartiles. If the intention is to apply the IQR rule independently to likes, calculate it as `likes_Q3 - likes_Q1` before calculating the likes bounds.
- The cohort column is named `cohort month` in the notebook, but its values are created with `to_period('W')`, so the grouping is **weekly**, not monthly.
- The notebook's printed interpretations are observations recorded in the notebook. Results may differ if the input CSV files or preprocessing steps change.

## Output

When executed successfully, the notebook displays:

- Data-quality checks and summary statistics.
- A Matplotlib/Seaborn EDA dashboard.
- Interactive Plotly charts.
- Outlier counts and correlation heatmap.
- Funnel stage conversion rates.
- Retention curves by category and country.
- Average trending lifespan by weekly cohort.
- A written content-strategy briefing.

## Scope and limitations

This notebook focuses on exploratory analysis of the supplied US, IN, and GB trending-video datasets. Its comparisons describe the records available in those files and should not automatically be interpreted as causal relationships or as representative of all YouTube activity.
