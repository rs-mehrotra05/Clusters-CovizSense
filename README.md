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
