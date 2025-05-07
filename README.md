- 👋 Hi, I’m @lakshaygola10
- 👀 I’m interested in ...projects related to Excel, python, SQL 
- 🌱 I’m currently learning ... advanced Excel, SQL 
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ... Dm me or mail me at lakshaygola06@gmail.com 
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
import pandas as pd
import matplotlib as mt
import folium as fo
dataset = pd.read_csv("/content/accident_prediction_india.csv", encoding = 'latin-1')
show = dataset.head()
print("Information about the first 5 are :")
print(show)
print(f"information about the{dataset.info()}")
print("\nMissing Values:")
print(dataset.isnull().sum())'
dataset = dataset[dataset["Road Type"] == "National Highway"]
print(dataset)
dataset = dataset.drop("Day of Week", axis=1)
print(dataset)

dataset = dataset.drop(["Time of Day","Accident Severity","Number of Vehicles Involved", "Vehicle Type Involved", "Number of Casualties","Number of Fatalities"],axis = 1 )
print(dataset)
dataset = dataset.drop(["Month", "Weather Conditions","Driver License Status","Driver Gender","Traffic Control Presence","Lighting Conditions","Road Condition"], axis = 1)
print(dataset)
dataset = dataset.drop(["Driver Age","Alcohol Involvement"], axis = 1)
print(dataset)
data = dataset.describe()
print(data)
y = dataset["State Name"]
x = dataset[["Road Type", "Speed Limit (km/h)", "Accident Location Details","Weather Conditions", "Road Condition"]]
from sklearn.model_selection import train_test_split
x_train, x_test, y_train, y_test = train_test_split(x,y, test_size=0.2, random_state=42)
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.preprocessing import OneHotEncoder

# ... (your previous code to load and preprocess the dataset) ...

# Selecting features and target variable
y = dataset["State Name"]
x = dataset[["Road Type", "Speed Limit (km/h)", "Accident Location Details","Weather Conditions", "Road Condition"]]

# Create a OneHotEncoder object
encoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore') # sparse=False for dense output

# Fit the encoder on the categorical features and transform them
encoded_features = encoder.fit_transform(x[['Road Type', 'Accident Location Details', 'Weather Conditions', 'Road Condition']])

# Create a DataFrame from the encoded features
encoded_df = pd.DataFrame(encoded_features, columns=encoder.get_feature_names_out(['Road Type', 'Accident Location Details', 'Weather Conditions', 'Road Condition']))

# Concatenate the encoded features with the numerical feature ('Speed Limit (km/h)')
x = pd.concat([x[['Speed Limit (km/h)']], encoded_df], axis=1)


# Split data into training and testing sets
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2, random_state=42)

# Initialize and train the Decision Tree model
model = DecisionTreeClassifier()
model.fit(x_train, y_train)
location_counts = dataset.groupby(['Road Type', 'State Name']).size().reset_index(name='count')
print(location_counts.head())
#heat map
from folium.plugins import HeatMap

# Sample data (replace with your dataset)
accident_data = pd.DataFrame({
    'latitude': [19.0760, 28.7041],  # Mumbai, Delhi
    'longitude': [72.8777, 77.1025],
    'count': [150, 200]  # Accident frequencies
})

# Convert counts to integers
accident_data['count'] = accident_data['count'].astype(int)

# Create heatmap data [[lat, lon] * count]
heat_data = []
for _, row in accident_data.iterrows():
    # Convert row['count'] to int to fix the error
    heat_data.extend([[row['latitude'], row['longitude']]] * int(row['count']))

# Generate map
m = folium.Map(location=[22.5937, 78.9629], zoom_start=5)
HeatMap(heat_data, radius=15).add_to(m)
m.save('accident_heatmap.html')
try:
    display(m)  
except:
    pass
!pip install folium geopandas
import pandas as pd
import geopandas as gpd
import folium

# Load India GeoJSON from reliable source
india_geojson_url = "https://raw.githubusercontent.com/geohacker/india/master/state/india_state.geojson"

# Load accident data (replace with your actual path)
try:
    accident_data = pd.read_csv('/content/accident_prediction_india.csv')
    # Aggregate data to get accident counts by state
    accident_counts = accident_data.groupby('State Name')['State Name'].count().reset_index(name='Accident_Count') 
except FileNotFoundError:
    # Sample data if file missing
    accident_counts = pd.DataFrame({
        'State Name': ['Maharashtra', 'Karnataka', 'Tamil Nadu'],
        'Accident_Count': [1500, 1200, 900]
    })

# Create interactive map
m = folium.Map(location=[22.5937, 78.9629], zoom_start=5)

# Add choropleth layer
folium.Choropleth(
    geo_data=india_geojson_url,
    name='Accident Hotspots',
    data=accident_counts,
    columns=['State Name', 'Accident_Count'],
    key_on='feature.properties.NAME_1',
    fill_color='YlOrRd',
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name='Accident Frequency'
).add_to(m)

# Add state labels
folium.GeoJson(
    india_geojson_url,
    style_function=lambda x: {'fillOpacity': 0, 'color': 'black', 'weight': 0.5},
    tooltip=folium.features.GeoJsonTooltip(
        fields=['NAME_1'],
        aliases=['State: ']
    )
).add_to(m)

# Add layer control
folium.LayerControl().add_to(m)

# Display in Colab
display(m)
