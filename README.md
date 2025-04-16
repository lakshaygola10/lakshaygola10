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
x = dataset[["Road Type", "Speed Limit (km/h)", "Accident Location Details"]]
from sklearn.model_selection import train_test_split
x_train, x_test, y_train, y_test = train_test_split(x,y, test_size = 0.2, random_state=42)
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit = x_train,y_train
location_counts = dataset.groupby(['Road Type', 'State Name']).size().reset_index(name='count')

# Now location_counts is defined and ready to use!
print(location_counts.head())
import folium
m = folium.Map(location=[22.5937, 78.9629], zoom_start=5, tiles='cartodbpositron')
folium.Choropleth(
    geo_data=india_geojson,
    name='Accident Count',
    data=accident_data,
    columns=['State Name', 'count'],
    key_on='feature.properties.NAME_1',  # Correct property name
    fill_color='YlOrRd',
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name='Accidents on National Highways',
    highlight=True
).add_to(m)

folium.GeoJson(
    india_geojson,
    style_function=lambda feature: {
        'fillColor': '#ffffff',
        'color': 'black',
        'weight': 0.1,
        'fillOpacity': 0
    },
    tooltip=folium.features.GeoJsonTooltip(
        fields=['NAME_1'],
        aliases=['State: '],
        localize=True
    )
).add_to(m)
m.save('national_highway_accidents.html')
try:
    from IPython.display import display, HTML
    display(m)
except ImportError:
    pass

# --- Download link for Google Colab ---
try:
    import google.colab
    from google.colab import files
    files.download('national_highway_accidents.html')
except ImportError:
    pass





<!---
lakshaygola10/lakshaygola10 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
