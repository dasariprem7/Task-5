# Task-5
Analyzing Traffic Accident Data
Analyzing traffic accident data to identify patterns related to road conditions, weather, and time of day can provide valuable insights into factors contributing to accidents. Below is a step-by-step guide for analyzing the dataset, including visualizations to identify accident hotspots and contributing factors.

1. Load the Dataset
First, download the dataset from the provided link and load it into a DataFrame. For this example, I'll assume the dataset is named US_Accidents_Dec21.csv.
import pandas as pd

# Load the dataset
url = 'https://www.kaggle.com/code/harshalbhamare/us-accident-eda/download'
data = pd.read_csv(url)

# Display the first few rows of the dataset
print(data.head())

2. Data Preprocessing
Basic Exploration
Examine the dataset to understand its structure and clean any inconsistencies.
# Display basic information about the dataset
print(data.info())

# Check for missing values
print(data.isnull().sum())

# Display the first few rows to understand the columns
print(data.head())

Data Cleaning
Handle missing values, and convert date and time columns into a more usable format.
# Drop rows with missing values in critical columns
data = data.dropna(subset=['Severity', 'Start_Time', 'Weather_Condition', 'Road_Condition'])

# Convert 'Start_Time' to datetime format
data['Start_Time'] = pd.to_datetime(data['Start_Time'])

# Extract additional time-related features
data['Hour'] = data['Start_Time'].dt.hour
data['Day_of_Week'] = data['Start_Time'].dt.day_name()
data['Month'] = data['Start_Time'].dt.month_name()

3. Analyze Accident Patterns
Analyze Accidents by Time of Day and Day of Week
Visualize how accidents vary by hour and day of the week.
import matplotlib.pyplot as plt
import seaborn as sns

# Set the style for seaborn
sns.set(style="whitegrid")

# Accidents by Hour of the Day
plt.figure(figsize=(12, 6))
sns.countplot(x='Hour', data=data, palette='viridis')
plt.title('Number of Accidents by Hour of the Day')
plt.xlabel('Hour of the Day')
plt.ylabel('Number of Accidents')
plt.show()

# Accidents by Day of the Week
plt.figure(figsize=(12, 6))
sns.countplot(x='Day_of_Week', data=data, palette='viridis', order=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'])
plt.title('Number of Accidents by Day of the Week')
plt.xlabel('Day of the Week')
plt.ylabel('Number of Accidents')
plt.show()

Analyze Accidents by Weather and Road Conditions
Visualize the impact of weather and road conditions on the number of accidents.
# Accidents by Weather Condition
plt.figure(figsize=(14, 7))
sns.countplot(y='Weather_Condition', data=data, palette='viridis', order=data['Weather_Condition'].value_counts().index)
plt.title('Number of Accidents by Weather Condition')
plt.xlabel('Number of Accidents')
plt.ylabel('Weather Condition')
plt.show()

# Accidents by Road Condition
plt.figure(figsize=(14, 7))
sns.countplot(y='Road_Condition', data=data, palette='viridis', order=data['Road_Condition'].value_counts().index)
plt.title('Number of Accidents by Road Condition')
plt.xlabel('Number of Accidents')
plt.ylabel('Road Condition')
plt.show()

4. Identify Accident Hotspots
Visualize accident hotspots using geographical data. This requires latitude and longitude data, so we'll use a map visualization.
import folium

# Create a base map
map_center = [data['Latitude'].mean(), data['Longitude'].mean()]
accident_map = folium.Map(location=map_center, zoom_start=10)

# Add accident points to the map
for index, row in data.iterrows():
    folium.CircleMarker(
        location=[row['Latitude'], row['Longitude']],
        radius=2,
        color='red',
        fill=True,
        fill_color='red',
        fill_opacity=0.5
    ).add_to(accident_map)

# Save the map to an HTML file
accident_map.save('accident_map.html')

5. Correlation Analysis
Analyze the correlation between different features to understand how they contribute to accidents.
# Calculate the correlation matrix
corr_matrix = data[['Hour', 'Day_of_Week', 'Month', 'Weather_Condition', 'Road_Condition', 'Severity']].apply(lambda x: pd.factorize(x)[0]).corr()

# Plot the correlation heatmap
plt.figure(figsize=(12, 8))
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', fmt='.2f')
plt.title('Correlation Heatmap of Accident Factors')
plt.show()

*Summary
Data Preprocessing: Cleaned and prepared the dataset, including handling missing values and converting date/time columns.
Accident Patterns: Visualized accidents by time of day, day of the week, weather conditions, and road conditions.
Hotspot Analysis: Used geographical data to visualize accident hotspots on a map.
Correlation Analysis: Analyzed correlations between different factors contributing to accidents.





















