# Project Introduction
Analyze global COVID‑19 trends using Pandas for data manipulation and Plotly/Matplotlib for interactive and static visualizations. Typical goals:
Trend detection (cases, deaths, vaccinations over time)
Outlier detection (e.g., via log transformation and boxplots)
Country/continent comparisons and insights

# Data analysis and Manipulation
import plotly.graph_objs as go  
import plotly.io as pio 
import plotly.express as px 
import pandas as pd 

# Data Visualization
import matplotlib.pyplot as plt

# Importing Plotly
import plotly.offline as py
py.init_notebook_mode(connected=True)

# Initializing Plotly
pio.renderers.default = 'colab'

# Importing Dataset1
# Load dataset from Our World in Data
url = 'https://covid.ourworldindata.org/data/owid-covid-data.csv'
df = pd.read_csv(url)

# Display first few rows
df.head()



#cleaning and Handling Missing Values
# Select relevant columns
df = df[['continent', 'location', 'date', 'total_cases', 'new_cases', 'total_deaths',
         'new_deaths', 'total_vaccinations', 'people_fully_vaccinated']]

# Drop rows without continent
df.dropna(subset=['continent'], inplace=True)

# Fill missing numeric values with 0
df.fillna(0, inplace=True)

# Convert date to datetime
df['date'] = pd.to_datetime(df['date'])

#Feature Selection and Engineering

# Mortality rate (%)
df['mortality_rate'] = np.where(df['total_cases'] > 0,
                                df['total_deaths'] / df['total_cases'] * 100, 0)

# Vaccination rate
df['vaccination_rate'] = np.where(df['total_vaccinations'] > 0,
                                  df['people_fully_vaccinated'] / df['total_vaccinations'] * 100, 0)




#Ensuring Data Integrity & Consistency

# Remove negative values
df = df[(df['new_cases'] >= 0) & (df['new_deaths'] >= 0)]

# Drop duplicates
df = df.drop_duplicates()

# Check for nulls
assert df.isnull().sum().sum() == 0, "There are still missing values!"


#Summary Statistics and Insights

print(df.describe())

# Top countries by total cases
top_cases = df.groupby('location')['total_cases'].max().sort_values(ascending=False).head(5)
print("Top 5 countries with highest total cases:\n", top_cases)



#Identifying Patterns, Trends, and Anomalies

# Global aggregation
global_df = df.groupby('date').sum(numeric_only=True).reset_index()

# Global new cases trend
fig = px.line(global_df, x='date', y='new_cases', title='Global Daily New COVID-19 Cases')
fig.show()

# Global new deaths trend
fig = px.line(global_df, x='date', y='new_deaths', title='Global Daily New COVID-19 Deaths')
fig.show()



#Handling Outliers & Data Transformations

# Log transform to manage skewness
df['log_new_cases'] = np.log1p(df['new_cases'])

# Box plot to visualize outliers
fig = px.box(df, y='log_new_cases', title='Log-Transformed New Cases (Outlier Detection)')
fig.show()


#Initial Visual Representations

#Top 10 Countries By total Death

top_deaths = df.groupby('location')['total_deaths'].max().sort_values(ascending=False).head(10).reset_index()

fig = px.bar(top_deaths, x='location', y='total_deaths',
             title='Top 10 Countries by Total COVID-19 Deaths',
             color='total_deaths', color_continuous_scale='reds')
fig.show()


#Vaccination distribution (top 5)
latest_date = df['date'].max()
latest_df = df[df['date'] == latest_date]

top_vaccinated = latest_df.sort_values(by='people_fully_vaccinated', ascending=False).head(5)

fig = px.pie(top_vaccinated, names='location', values='people_fully_vaccinated',
             title='People Fully Vaccinated (Top 5 Countries)')
fig.show()

## Review 2
# Data Visualization

#Positive Linear Realtionship

# scatterplot of "total_cases" and "new_cases"
sns.regplot(x="total_cases", y="new_cases",data=df, color='red')
plt.ylim(0,)
sns.regplot(x='total_deaths', y='new_deaths', data=df)

# scatterplot total_cases vs total_deaths
plt.scatter(df['total_deaths'], df['total_cases'])

# Histogram of total_deaths
plt.hist(df['total_deaths'])

# Histogram of new_cases
plt.hist(df['new_cases'])

# Barchart Continents vs number of patients in each continent
bardata= pd.DataFrame({'cont': ['Africa' , 'Asia', 'Oceania', 'Europe', 'North America', 'South America'], 'Num':[95058,79476,31035,92953,67351,24120]})
bardata['cont']
plt.bar(bardata['cont'],bardata['Num'],color='hotpink')
plt.title("Bar chart,  Continents Vs No of pateints in each continent")
plt.xlabel("Continents")
plt.xticks(list(bardata['cont']))
plt.ylabel("Frequency")
plt.yticks(bardata['Num'])
plt.show()

# Piechart
plt.pie(bardata['Num'])
plt.pie(bardata['Num'], labels = bardata['cont'])
plt.show()

# How to Run

A. Setup

1. Clone the repo and enter its directory.
2. Create a virtual environment:
python3 -m venv venv
source venv/bin/activate   # (Linux/macOS)
venv\Scripts\activate      # (Windows)
3. Install dependencies:
pip install -r requirements.txt
requirements.txt should include:
pandas, numpy, plotly, matplotlib, seaborn

B. Download Data

Ensure data/owid-covid-data.csv exists — either include it in the repo or run:
import pandas as pd
df = pd.read_csv('https://covid.ourworldindata.org/data/owid-covid-data.csv')
df.to_csv('data/owid-covid-data.csv', index=False)

C. Execute Notebook

Run in Jupyter or Colab:
jupyter notebook notebooks/covid_analysis.ipynb

D. Run Python Scripts

For modular tasks:
python src/data_processing.py
python src/visualization.py

4. Data Analysis & Visualization Steps

Data Cleaning
Keep key columns: continent, date, cases, deaths, vaccinations
Drop NaNs from continent, fill numeric with zeros
Convert date to datetime format

Feature Engineering
mortality_rate = total_deaths / total_cases × 100
vaccination_rate = fully_vaccinated / total_vaccinations × 100

Filtering
Remove negative entries

Drop duplicates
Assert no missing values remain

Exploration & Insights

Summary statistics (df.describe())
Top 5 countries by total cases, top 10 by deaths
Time-Series Visualizations
Global new cases/deaths trends using px.line

Outlier Handling
Log-transform new_cases (log_new_cases = log1p(new_cases))
Use box plots for outlier visualization

Comparisons
Bar plots (top countries by deaths, continent-wise plots)
Pie charts (vaccination distribution among leading countries)
Scatter and regression plots (total vs new cases/deaths)

Histograms for distribution trends





