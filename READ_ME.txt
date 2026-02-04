# Mobile Phone Activity Analysis - Milan CDR Dataset

Author: Emile Lucky Muhigira  
Course: Mobile Big Data Analytics and Management  
Institution: Carnegie Mellon University - Africa  
Date: February 4th, 2026

--------------------------

## Overview of Approach

This project analyzes three days of Call Detail Records (CDR) from Milan, Italy (November 2, 4, 6, 2013), containing 6,564,031 records of mobile phone activity across 10,000 spatial grid cells. The analysis examines temporal patterns, spatial distributions, and behavioral differences between domestic and international communications.

I loaded three CSV files and concatenated them into a single dataframe, then added temporal features (date, hour) and aggregate columns (total_sms, total_calls, total_internet). The primary challenge was handling missing values, which affected 89.6% of records - typical for sparse CDR data where most grid cells have zero activity for specific communication types during any given hour.

For statistical analysis, I explicitly used NumPy functions (np.mean(), np.sum(), np.corrcoef()) rather than pandas methods, converting dataframes to NumPy arrays with .values before calculations. I separated domestic (Italy, country code 39) from international communications to enable comparative analysis. Temporal patterns were analyzed by grouping data by hour to identify peak and low activity periods.

-------------------------

## Key Decisions

1. Mean Imputation for Missing Values  

Out of 6,564,031 records, 5,880,441 (89.6%) had at least one missing value. I filled missing numeric values with column means rather than zero-filling or dropping records. This preserves overall activity distributions without introducing bias toward zero, which would severely underestimate network activity. SMS outgoing had the most missing values (5M+), followed by call incoming, SMS incoming, call outgoing, and internet activity. Mean imputation maintains the full spatial and temporal coverage while using the large sample size to provide robust estimates.

2. Domestic vs International Classification  

I defined domestic as country code 39 (Italy) and international as all other codes. Since the dataset was collected in Milan, using Italy's international dialing code provides a clear binary classification. This enabled clean separation for comparing communication patterns and calculating percentages of domestic vs international activity.

3. Grid-Level Aggregation for Correlation
  
I summed SMS and call volumes by CellID before computing correlation rather than using hour-level data. This approach reveals whether areas with high SMS activity also have high call activity (spatial patterns) rather than just temporal coincidence. Aggregating at the spatial level reduces temporal noise and focuses on location-based communication behavior.

4. Explicit NumPy Usage
  
The assignment required NumPy for statistical comparisons. I converted pandas Series to NumPy arrays using .values, then applied NumPy functions exclusively: np.mean(), np.std(), np.median() for descriptive statistics, np.sum() for aggregations, and np.corrcoef() for correlation analysis. This demonstrates proficiency with array-based computations beyond pandas wrapper methods.

5. Temporal Boundaries
  
I defined daytime as 6 AM to 8 PM (hours 6-19) and nighttime as 8 PM to 6 AM (hours 20-23, 0-5). These boundaries align with typical work schedules and waking hours in urban areas, capturing the transition between business and leisure time.

------------------------------

## Summary of Key Findings

Dataset Characteristics: The combined dataset spans 6,564,031 records across 10,000 grid squares covering Milan. Communications involve 302 different countries, demonstrating the international nature of a major European city. The high proportion of missing values (89.6%) reflects typical CDR sparsity where most cells have zero activity for many communication types during most hours.

Temporal Patterns: Peak activity occurs at 5 PM (Hour 17), representing the end of the workday when people coordinate evening plans and begin commutes - maximum overlap between business and personal communications. Lowest activity occurs at 3 AM (Hour 3) during the deep sleep period. The data shows strong diurnal patterns with gradual morning increases, a midday plateau, late afternoon peak, and evening decline.

Communication Behavior: Domestic (Italian) communications vastly outnumber international calls and SMS, as expected for local mobile network usage. International communications show different temporal patterns with peaks at slightly different hours, possibly reflecting business connections across time zones. The analysis of incoming vs outgoing international calls reveals Milan's role as either a receiver of international calls (tourism/business destination) or a source of calls abroad.

Spatial Patterns: I found positive correlation between SMS and call volumes at the grid level, indicating that areas with high text messaging activity also have high voice call activity. This suggests consistent "communication hubs" corresponding to business districts, transportation centers, and dense residential areas where both communication modes concentrate.

Statistical Distributions: Descriptive statistics revealed high variability across hours. The median being lower than the mean indicates right-skewed distributions where certain hours and grid cells (train stations, airports, city centers) generate disproportionate traffic while most locations show moderate activity levels. This pattern is typical of urban telecommunications data.

The results validated against expected behavioral patterns: the 5 PM peak and 3 AM trough align with published research on urban telecommunications, confirming the analysis reflects genuine human behavior rather than data artifacts. All percentages summed correctly to 100%, and correlation coefficients fell within the valid [-1, 1] range.

-----------------------------------

## Technical Implementation

Tools: Python 3.x, pandas (data manipulation), NumPy (statistical computations), Jupyter Notebook  
Key Operations: Data concatenation, mean imputation, group by aggregations, NumPy array conversions  
Validation: Verified record counts, confirmed zero post-imputation missing values, checked statistical ranges  

-----------------------------------

## Challenges and Solutions

Challenge: 90% of records had missing values - standard dropna() would eliminate most data.  
Solution: Mean imputation preserved full spatial-temporal coverage with statistically reasonable values.

Challenge: Processing 6.5 million records efficiently.  
Solution: Used vectorized pandas operations, aggregated before computing statistics.

Challenge: Temporal filtering for daytime/nighttime with midnight wraparound.  
Solution: Boolean logic with OR operator: (hour >= 20) | (hour < 6).

------------------------------------

## Repository Structure

```
assignment-1-cdr-analysis/
├── Assignment_1.ipynb          # Main analysis notebook
├── README.md                   # This file
└── data/                       # Dataset files 
    ├── sms-call-internet-mi-2013-11-02.csv
    ├── sms-call-internet-mi-2013-11-04.csv
    └── sms-call-internet-mi-2013-11-06.csv
```

---

## How to Reproduce

1. Download dataset from: https://www.kaggle.com/datasets/marcodena/mobile-phone-activity
2. Place CSV files in data/ directory
3. Update data_path variable in notebook
4. Run all cells sequentially

---

## References

Barlacchi, G., et al. (2015). "A multi-source dataset of urban life in the city of Milan and the Province of Trentino." *Scientific Data*, 2, 150055.

---

Contact: emuhigira@hotmail.com  
GitHub: [https://github.com/Emile-Lucky-Muhigira/Mobile-Big-Data-Analytics-and-Management]

