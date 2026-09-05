# Spam Email Detection — EDA, Feature Selection & Dimensionality Reduction

## Objective
Exploratory Data Analysis, data cleaning, feature selection and dimensionality reduction (PCA) on the classic Spambase dataset, to understand which email characteristics are most associated with spam and how the "curse of dimensionality" affects this kind of problem. Developed for the Data Exploration & Preparation module (CA2), L7 Diploma in Data Analytics, CCT College Dublin.

## Dataset
- **Source:** [UCI Machine Learning Repository — Spambase Dataset](https://archive.ics.uci.edu/dataset/94/spambase) (Hopkins, Reeber, Forman & Suermondt, Hewlett-Packard Labs, 1999).
- **Size:** 4,601 emails × 58 columns (57 features + 1 target).
- **Features:** 48 word-frequency percentages (e.g. `word_freq_free`, `word_freq_your`), 6 character-frequency percentages, and 3 features describing runs of capital letters (average/longest/total length).
- **Target:** `is_spam` — 1,813 spam (39.4%) vs 2,788 non-spam (60.6%).

## Data Cleaning & Preparation
- **Duplicates:** none found.
- **Missing values:**
  - 9 features had under 1% nulls — rows dropped directly.
  - `word_freq_labs` had 237 nulls (~5.2%) — filled with the **median**, chosen due to a right-skewed distribution with outliers.
  - `word_freq_our`, `word_freq_000` and `word_freq_hpl` contained non-numeric placeholder values (`'???'`, `'none'`) that were converting the columns to text; converted to numeric (`errors='coerce'`), which produced 1 null each — those 3 rows were dropped.
- **Dropped `Unnamed: 0`** (leftover index column, no analytical value).
- **Target encoding:** `is_spam` converted from True/False to 1/0.
- **Final cleaned dataset:** 4,514 rows (2,721 non-spam / 1,793 spam — ratio close to the original).
- **Outliers were intentionally kept**, not removed — the reasoning documented in the notebook is that since the goal is binary spam/not-spam prediction, extreme values (e.g. an email in ALL CAPS) can be genuinely useful signal rather than noise to eliminate.

## Feature Selection
`SelectKBest` (chi², k=8) ranked features against `is_spam`. Top 8:

| Rank | Feature | Score |
|---|---|---|
| 1 | capital_run_length_total | 362,530.78 |
| 2 | capital_run_length_longest | 153,343.32 |
| 3 | capital_run_length_average | 10,592.83 |
| 4 | word_freq_george | 2,184.97 |
| 5 | word_freq_hp | 1,547.13 |
| 6 | word_freq_your | 1,180.80 |
| 7 | word_freq_free | 855.32 |
| 8 | word_freq_hpl | 743.44 |

**Insight:** the three strongest predictors are all about **capital-letter usage patterns**, not specific words — suggesting that *how* an email is written (e.g. excessive capitalisation) is a stronger spam signal than any single keyword.

## Key Visualisations
- Histograms and boxplots for each of the top 8 features, showing strongly right-skewed distributions.
- Correlation heatmap of the top 8 features against `is_spam`.
- Cumulative explained-variance plots for PCA, comparing **normalised vs non-normalised** data.

## Dimensionality Reduction (PCA)
- Data was standardised (`StandardScaler`) before PCA, since the unscaled features have very different scales (percentages vs raw counts), which would otherwise distort the result.
- On the **normalised** data, **50 of the 57 components** were needed to reach 99.5% cumulative explained variance — showing this dataset is genuinely high-dimensional; dimensionality reduction alone only trims 7 features at that variance threshold.
- On the **non-normalised** data, a couple of high-scale features (the capital-letter run-length stats) dominate the variance almost entirely — a clear demonstration of why scaling matters before PCA.

## Results & Limitations
- **Feature importance ≠ correlation:** the notebook explicitly notes that the top chi² features aren't necessarily the most *linearly correlated* with `is_spam` individually — they may matter through interaction with other features rather than a direct relationship.
- **Curse of dimensionality:** explained and discussed in the notebook — with many features (even after selection), models risk struggling to find reliable patterns and generalising well.
- ⚠️ **No final classifier was trained.** This project's scope stops at EDA, cleaning, feature selection and PCA — it does not include a trained model or a classification accuracy/F1 score. If the goal is a "spam filter," that's the natural next step to add.
- **Minor code note:** the cell comparing "non-normalised" PCA drops rows instead of the `is_spam` column (a `.drop()` argument issue), so that comparison isn't a perfectly clean apples-to-apples test against the normalised version. Worth a quick fix (`df.drop(columns=['is_spam'])`) for correctness.

## Conclusion
Formatting-based signals — especially heavy use of capital letters — are stronger indicators of spam in this dataset than any individual suspicious word. A production spam filter built from this data should weight structural/formatting features alongside keyword frequency, not rely on keywords alone.

## Files in this repository

| File | Description |
|---|---|
| [`CA2_Spam.ipynb`](./CA2_Spam.ipynb) | Full analysis notebook — EDA, cleaning, feature selection, PCA and curse-of-dimensionality discussion |
| [`spambase.csv`](./spambase.csv) | Source dataset (4,601 emails) |
| [`spambase_documentation.txt`](./spambase_documentation.txt) | Original UCI dataset documentation and attribute descriptions |
