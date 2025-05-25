# Clusters-CovizSense
This project analyzes global COVID-19 data using Python and Plotly Express, focusing on data cleaning, feature engineering, and trend visualization. It highlights key insights through interactive charts and handles outliers for accurate analysis.
___________________________________________________________________________________________________________________________________________________________________		
Project Overview
This project analyses global COVID-19 data using Python and Plotly Express, focusing on data cleaning, feature engineering, and trend visualization. It highlights key insights through interactive charts and handles outliers for accurate analysis.
____________________________________________________________________________________________________________________________________________________________________
Data Cleaning and Handling Missing Values
•	Missing Value Treatment: Employed forward-fill (ffill) method to address missing entries, ensuring continuity in time-series data.
•	Data Type Corrections: Converted the 'Date' column to a date time format for accurate temporal analysis.
•	Standardization: Standardized country and region names to maintain consistency across the dataset.
____________________________________________________________________________________________________________________________________________________________________
Feature Selection and Engineering 
•	Derived Metrics:
o	Active Cases: Calculated as Confirmed - Recovered - Deaths.
o	Mortality Rate: Computed as Deaths / Confirmed.
o	Recovery Rate: Computed as Recovered / Confirmed.
•	Temporal Features: Extracted features like week number and month to capture temporal trends.
•	Categorical Encoding: Ensured categorical variables are in appropriate formats for analysis.
____________________________________________________________________________________________________________________________________________________________________
Ensuring Data Integrity and Consistency 
•	Duplicate Removal: Identified and removed duplicate records to prevent skewed analyses.
•	Consistency Checks: Ensured uniformity in country and region names and validated date sequences.
•	Range Validation: Checked for logical inconsistencies, such as negative case counts.
____________________________________________________________________________________________________________________________________________________________________
Summary Statistics and Insights
•	Descriptive Statistics: Computed mean, median, and standard deviation for key metrics.
•	Top Affected Regions: Identified countries with the highest case counts and mortality rates.
•	Temporal Trends: Analyzed the progression of cases over time to identify peaks and declines.
____________________________________________________________________________________________________________________________________________________________________
Identifying Patterns, Trends, and Anomalies 
•	Trend Analysis: Visualized the rise and fall of cases across different regions and timeframes.
•	Anomaly Detection: Spotted irregularities, such as sudden spikes or drops in case numbers, prompting further investigation.
•	Correlation Studies: Examined relationships between variables, like testing rates and confirmed cases.
____________________________________________________________________________________________________________________________________________________________________
Handling Outliers and Data Transformations
•	Outlier Detection: Utilized statistical methods to identify anomalous data points.
•	Data Transformation: Applied log transformations to normalize skewed distributions.
•	Impact Assessment: Evaluated the influence of outliers on overall trends and adjusted analyses accordingly.
____________________________________________________________________________________________________________________________________________________________________
Initial Visual Representation of Key Findings
•	Interactive Line Charts: Depicting daily new cases and deaths over time.
•	Choropleth Maps: Visualizing the geographical spread and intensity of the pandemic.
•	Bar Charts: Comparing metrics across different countries and regions.
