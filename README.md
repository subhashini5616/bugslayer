
# Project Title

A brief description of what this project does and who it's for

Python is a highly powerful general-purpose programming language that can be easily learned and provides data scientists a wide variety of tools and packages. 


Steps involved in Data Analysis:

1. Importing required packages
2. Gathering Data

3. Transforming Data to our needs (Data Wrangling)

4. Exploratory Data Analysis (EDA) and Visualization

Step — 1: Importing required Packages
Importing our required packages is the starting point of all data analysis programming in python. As I’ve said, python provides a wide variety of packages for data scientists and in this analysis, I used python’s most popular data science packages Pandas and NumPy for Data Wrangling and EDA. When coming to Data Visualization, I used python’s interactive packages Plotly and Matplotlib. It’s very simple to import packages in python code:

    import pandas as pd
    import matplotlib.pyplot as plt
    import plotly.express as px
    import numpy as np
    import plotly
    import plotly.graph_objects as go
    from plotly.subplots import make_subplots


Step — 2: Gathering Data
For a clean and perfect data analysis, the foremost important element is collecting quality Data. For this analysis, I’ve collected many data from various sources for better accuracy.

    import requests

    url_request = requests.get("https://services1.arcgis.com/0MSEUqKaxRlEPj5g/arcgis/rest/services/Coronavirus_2019_nCoV_Cases/FeatureServer/1/query?where=1%3D1&outFields=*&outSR=4326&f=json")
    url_json = url_request.json()
    df = pd.DataFrame(url_json['features'])



Step — 3: Data Wrangling
Data Wrangling is a process where we will transform and clean our data to our needs. We can’t analyze with our raw extracted data. So, we have to transform the data to proceed with our analysis. Here’s the code for our Data Wrangling:

    import datetime as dt
    data_list = df['attributes'].tolist()
    data = pd.DataFrame(data_list)
    data.set_index('OBJECTID')
    data = data[['Province_State','Country_Region','Last_Update',  'Lat','Long_','Confirmed','Recovered','Deaths','Active']]
    data.columns = ('State','Country','Last Update','Lat','Long','Confirmed','Recovered','Deaths','Active')
    data['State'].fillna(value = '', inplace = True)
    data

    def convert_time(t):
    t = int(t)
    return dt.datetime.fromtimestamp(t)

    data = data.dropna(subset = ['Last Update'])
    data['Last Update'] = data['Last Update']/1000
    data['Last Update'] = data['Last Update'].apply(convert_time)
    data

    Step — 4: Exploratory Data Analysis and Data Visualization
This process is quite long as it is the heart and soul of data analysis. So, I’ve divided this process into three steps:

a. Ranking countries and provinces (based on COVID-19 aspects)

b. Time Series on COVID-19 Cases

c. Classification and Distribution of cases

i) Top 10 Confirmed Cases Countries:
# a. Top 10 Confirm Cases Countries(Bubble Plot)

    top10_confirmed = pd.DataFrame(data.groupby('Country')['Confirmed'].sum().nlargest(10).sort_values(ascending = False))
    fig1 = px.scatter(top10_confirmed, x = top10_confirmed.index, y = 'Confirmed', size = 'Confirmed', size_max = 120,
    color = top10_confirmed.index, title = 'Top 10     Confirmed Cases Countries')
    fig1.show()
    

ii) Top 10 Death Cases Countries:

# b. Top 10 deaths countries (h-Bar plot)

top10_deaths = pd.DataFrame(data.groupby('Country')['Deaths'].sum().nlargest(10).sort_values(ascending = True))
fig2 = px.bar(top10_deaths, x = 'Deaths', y = top10_deaths.index, height = 600, color = 'Deaths', orientation = 'h',
            color_continuous_scale = ['deepskyblue','red'], title = 'Top 10 Death Cases Countries')
fig2.show()

iii) Top 10 Recovered Cases Countries:

# c. Top 10 recovered countries (Bar plot)

top10_recovered = pd.DataFrame(data.groupby('Country')['Recovered'].sum().nlargest(10).sort_values(ascending = False))
fig3 = px.bar(top10_recovered, x = top10_recovered.index, y = 'Recovered', height = 600, color = 'Recovered',
             title = 'Top 10 Recovered Cases Countries', color_continuous_scale = px.colors.sequential.Viridis)
fig3.show()# bugslayer
