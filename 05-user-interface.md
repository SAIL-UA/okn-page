---
layout: default
title: "User Interface"
nav_order: 5
---

# User Interface

![OKNAPP](media/oknapp.png)

The app interface has three main components: a real-time map, a chatbot interface, and clinic information query boxes. When a user enters information into the chatbot, it classifies the questions into different categories and runs the appropriate LLM (Large Language Model) routing for each type of question. The map zooms in on the relevant location for questions associated with geolocated data in the database and displays the related clinic and services. The clinic details, including reviews and services, are retrieved from Google Maps and the clinic's website, which is crawled for information. This data is added to the database for future queries, eliminating the need to fetch it from Google Maps again.

## Tools

We used Dash Plotly to construct the web app. The source code can be found at [Shengting Cao repo](https://github.com/scao7/OKN-APP/).

### Setting Up the Environment

1. Create a new environment:
   ```
   conda create -n OKN python=3.10
   ```

2. Install Dash:
   ```
   pip install dash
   ```

3. Install pandas:
   ```
   pip install pandas
   ```

### Useful Links

- Dash Template: [Dash Gallery Portal](https://dash.gallery/Portal/)
- Theme Documentation: [Dash Theme Explorer](https://hellodash.pythonanywhere.com/theme-explorer/gallery)

### Project Layout

```
OKN/
  ├── assets/
  ├── Datasets/  # For survey data
  ├── pages/
  ├── app.py
  └── README.md
```

### Multipage Design Reference

Refer to Dash's documentation for [Multipage Design](https://dash.plotly.com/urls).

### Mapbox Styles

To customize the map style, use the following in `fig.update()`:
```
"open-street-map", "carto-positron", "carto-darkmatter", 
"stamen-terrain", "stamen-toner", "stamen-watercolor", 
"white-bg", "basic", "streets", "outdoors", 
"light", "dark", "satellite", "satellite-streets"
```

### GeoJSON Resources

- USDA Rural-Urban Continuum Codes (2013): [USDA Link](https://www.ers.usda.gov/data-products/rural-urban-continuum-codes/)
- USA State GeoJSON: [GitHub Link](https://github.com/PublicaMundi/MappingAPI/blob/master/data/geojson/us-states.json)
- USA County GeoJSON: [GitHub Link](https://gist.github.com/sdwfrost/d1c73f91dd9d175998ed166eb216994a#file-counties-geojson)

## PostgreSQL Setup

1. Install the required libraries:
   ```
   pip install dash psycopg2 sqlalchemy
   ```

2. Connect to the database:
   ```
   postgresql+psycopg2://username:password@host:port/database
   ```

3. Set environment variables for passwords:
   ```python
   import os
   os.environ['okn_database'] = "password"
   os.environ['OPENAI_API_KEY'] = "password"
   ```

   Or set them in your system environment variables and restart the terminal.

## Table Extraction

1. Install `tabula-py` for extracting tables from PDFs:
   ```
   pip install tabula-py
   ```

2. Java is required to run `tabula-py`: [Java Download](https://www.oracle.com/java/technologies/downloads/#jdk22-windows)

3. Install `jpype1`:
   ```
   pip install jpype1
   ```

Alternatively, use `camelot-py` which is less dependent on Java:
```
pip install camelot-py[cv]
pip install 'PyPDF2<3.0'
```

### Example Usage for Table Extraction

```python
import camelot

pdf_path = "path/to/your/pdf_file.pdf"
tables = camelot.read_pdf(pdf_path, pages='all')

for i, table in enumerate(tables):
    print(f"Table {i}")
    table.to_csv(f"table_{i}.csv")
```

## Geolocation with `geopy`

We used `geopy` to retrieve latitude and longitude for addresses. Documentation: [geopy](https://geopy.readthedocs.io/en/stable/).

Install `geopy`:
```
pip install geopy
```

### Example Usage

```python
from geopy.geocoders import Nominatim

geolocator = Nominatim(user_agent="geoapiExercises")
address = "1600 Amphitheatre Parkway, Mountain View, CA"
location = geolocator.geocode(address)

if location:
    print(f"Latitude: {location.latitude}, Longitude: {location.longitude}")
else:
    print("Location not found")
```

For Google API, use the [Google Geocoding API](https://developers.google.com/maps/billing-and-pricing/billing#geocoding).

---

