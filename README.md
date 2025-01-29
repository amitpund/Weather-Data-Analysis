# Weather-Data-Analysis
I am building a live weather data analysis project using Python, PostgreSQL, Power BI, and APIs, and I have to deploy it on GitHub with a live page.

Data Analysis project Weather Data on live updated on daily basis by using of
TEHCNIC, API, PYTHON, POSTGRESQL, DASHBOARD IN POWER BI
That’s a great goal, Amit! Here’s a potential roadmap to help you build dashboards using 
Python, PostgreSQL, and Power BI:
Let’s focus on automating live data retrieval using an API. Here’s how we can approach it:
Step 1: Understand the API
• Most APIs require an API key (authentication) and return data in JSON or XML 
format.
• Example: Let’s use a public API like OpenWeatherMap to get weather data.
OpenWeatherMap API Example:
• API Endpoint: https://api.openweathermap.org/data/2.5/weather
• Parameters:
o q: City name (e.g., Mumbai).
o appid: Your API key (sign up here for free).
o units: Units (e.g., metric for Celsius).
Step 2: Install Required Libraries
Install the requests library:
pip install requests
Step 3: Python Script to Fetch Live Data
Here’s a script to fetch weather data and save it to a database:
import requests
import pandas as pd
from sqlalchemy import create_engine
import time
# API details
API_KEY = 'your_api_key_here’ # Replace with your OpenWeatherMap API key
BASE_URL = 'https://api.openweathermap.org/data/2.5/weather'
# Cities to track
cities = [‘Nagpur’, 'Mumbai', 'Delhi', 'Chennai', 'Kolkata', 'Bengaluru']
# PostgreSQL connection details
DB_URL = 'postgresql+psycopg2://username:password@localhost:5432/mydatabase'
# Function to fetch weather data
def fetch_weather(city):
 params = {'q': city, 'appid': API_KEY, 'units': 'metric'}
 response = requests.get (BASE_URL, params=params)
 if response.status_code == 200:
 data = response.json()
 return {
 'City': city,
 'Temperature': data['main’] ['temp'],
 'Humidity': data['main’] ['humidity'],
 'Weather': data['weather'][0] ['description'],
 'Timestamp': pd.Timestamp.now()
 }
 else:
 print (f"Failed to fetch data for {city}. Status code: {response.status_code}")
 return None
# Fetch data for all cities
def fetch_all_weather():
 weather_data = []
 for city in cities:
 data = fetch_weather(city)
 if data:
 weather_data.append(data)
 return weather_data
# Save data to PostgreSQL
def save_to_postgresql(data):
 engine = create_engine(DB_URL)
 df = pd.DataFrame(data)
 df.to_sql ('weather_data', engine, if_exists='append', index=False)
 print ("Data saved to PostgreSQL successfully!")
# Automation loop (fetch data every hour)
def automate_weather_fetch():
 while True:
 print ("Fetching weather data...")
 weather_data = fetch_all_weather()
 if weather_data:
 save_to_postgresql(weather_data)
 print ("Waiting for the next fetch...")
 time.sleep(3600) # Fetch every hour (3600 seconds)
# Start automation
if __name__ == '__main__':
 automate_weather_fetch()
Step 4: Create PostgreSQL Table
Before running the script, create the table in PostgreSQL:
CREATE TABLE weather_data (
 City VARCHAR (50),
 Temperature FLOAT,
 Humidity INT,
 Weather VARCHAR (50),
 Timestamp TIMESTAMP
);
How This Works
1. The script fetches live weather data for multiple cities using the API.
2. It processes the JSON response and extracts key details (temperature, humidity, 
weather).
3. Data is saved into a PostgreSQL table for further analysis.
4. The process repeats every hour (using time.sleep).
Next Steps
• Run the script and verify that data is being saved into PostgreSQL.
• Use Power BI to connect to the weather_data table and create live dashboards.
• Let me know if you'd like a Power BI integration example next! 
Let’s move on to integrating Power BI with the PostgreSQL database to create a live 
dashboard from the weather_data table. Here’s how to do it step by step:
Step 1: Set Up the PostgreSQL Connector in Power BI
1. Open Power BI Desktop.
2. Go to Home > Get Data > More.
3. Select PostgreSQL database and click Connect.
o If it’s your first time, you might need to install the Npgsql PostgreSQL 
connector. Power BI will prompt you with a link to download and install it.
Step 2: Connect to Your Database
1. In the connection dialog:
o Server: Enter your PostgreSQL server address (e.g., localhost for local, or IP for 
remote).
o Database: Enter the name of your database (e.g., mydatabase).
2. Click OK, and in the next step, provide your PostgreSQL username and password.
Step 3: Select the weather_data Table
1. Once connected, Power BI will list all tables in your database.
2. Select the weather_data table.
3. Click Load to bring the data into Power BI.
Step 4: Create Visualizations
Now that the data is in Power BI, you can start building visualizations:
1. Line Chart (Temperature Trends):
o Drag Timestamp to the X-axis and Temperature to the Y-axis.
o Add a slicer for the city column to filter by specific cities.
2. Bar Chart (Humidity Comparison):
o Drag City to the X-axis and Humidity to the Y-axis.
3. Card (Current Weather):
o Create a card to display the latest Weather description for a selected city.
o Use filters to show only the most recent Timestamp.
4. Map Visualization (Weather by Location):
o If you have geographic coordinates (latitude/longitude), use a map 
visualization to display weather conditions for each city.
Step 5: Refresh Data Automatically
To ensure your Power BI dashboard stays up to date:
1. Go to Home > Transform Data > Data Source Settings.
2. Enable Scheduled Refresh:
o Publish the dashboard to Power BI Service.
o Set up a refresh schedule (e.g., every hour) to match your Python script’s 
update frequency.
Step 6: Optional - Use Python Visuals in Power BI
If you want advanced visuals, you can use Python scripts directly in Power BI:
1. Go to Home > Get Data > Python Script.
2. Paste the script to fetch data and create visuals using libraries like matplotlib or 
plotly.
Next Steps
Let me know if you’d like help with:
• Setting up scheduled refresh in Power BI.
• Writing Python scripts for custom Power BI visuals.
• Enhancing the dashboard with advanced analytics.
1. Setting Up Scheduled Refresh in Power BI
This ensures your Power BI dashboard pulls the latest data from PostgreSQL automatically.
Steps:
1. Publish the Report to Power BI Service:
o In Power BI Desktop, go to File > Publish > Publish to Power BI Service.
o Sign in to your Power BI account and select a workspace.
2. Set Up a Gateway for PostgreSQL:
o Install the On-premises Data Gateway from here.
o Configure the gateway and connect it to your Power BI account.
3. Add the Data Source in Power BI Service:
o Go to Settings > Manage Gateways in Power BI Service.
o Add a new data source:
▪ Select the gateway.
▪ Enter your PostgreSQL connection details (server, database, username, 
password).
4. Schedule Refresh:
o In Power BI Service, go to your dataset.
o Click Schedule Refresh and set the frequency (e.g., hourly, daily).
2. Writing Python Scripts for Custom Power BI Visuals
Python allows you to create advanced custom visuals directly in Power BI.
Steps:
1. Enable Python in Power BI:
o Go to File > Options and Settings > Options.
o Under Python scripting, set the path to your Python executable (e.g., 
C:\Python39\python.exe).
2. Add a Python Visual in Power BI:
o In Power BI Desktop, go to the Visualizations pane and select the Python 
visual.
o Drag data fields into the Python visual fields pane.
3. Example: Create a Heatmap:
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
# Dataframe is auto generated by Power BI
dataset = dataset
# Pivot data for heatmap
pivot_data = dataset.pivot_table(index='City', columns='Timestamp', values='Temperature')
# Create heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(pivot_data, cmap='coolwarm', annot=True)
plt.title("Temperature Heatmap")
plt.show()
o Once the script runs, the heatmap will appear in your visual.
2. Advanced Visuals:
o Use libraries like plotly for interactive visuals (e.g., 3D plots, interactive maps).
3. Enhancing the Dashboard with Advanced Analytics
Advanced analytics can provide deeper insights into your data.
Ideas:
A. Trend Analysis
• Use DAX in Power BI to calculate moving averages or trends over time.
Moving Average = AVERAGEX (
 DATESINPERIOD('weather_data'[Timestamp], 
 LASTDATE('weather_data'[Timestamp]), 
 -7, 
 DAY),
 'weather_data'[Temperature]
)
B. Anomaly Detection
• Highlight unusual weather patterns:
o Add a DAX measure to flag outliers (e.g., temperatures above 40°C or below 
0°C).
C. Predictive Analysis
• Use Python in Power BI to forecast future temperatures
from fbprophet import Prophet
# Prepare data
data = dataset[['Timestamp', 'Temperature']].rename(columns={'Timestamp': 'ds', 
'Temperature': 'y'})
# Model
model = Prophet ()
model.fit(data)
# Forecast
future = model.make_future_dataframe(periods=30)
forecast = model.predict(future)
# Plot
model.plot(forecast)
plt.show()
D. Real-Time Dashboards
• Use Power BI’s DirectQuery mode to query PostgreSQL in real-time instead of 
importing data.
