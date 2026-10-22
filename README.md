# Delhi Traffic Prediction & OpenStreetMap | Live app: [SmartRoute](https://smartroute-delhi.streamlit.app/)

A machine learning-powered system to predict travel time. It visualizes live congestion, stop-to-stop delay, and optimal routes using interactive maps and charts.

---

## Project Description

This project leverages Large Language Models (LLMs) via Groq to enhance the experience of travel time prediction and traffic insights across Delhi. Using curated datasets, it integrates predictive modeling with an interactive Streamlit dashboard, featuring Folium-based maps, real-time travel estimations, and intuitive route visualizations for a smoother and smarter user experience.

<img width="1279" height="628" alt="Screenshot (120)" src="https://github.com/user-attachments/assets/f9da63d5-e1c8-4033-9392-5246c1982b2d" />

---

## Data Preprocessing & Feature Engineering

All GTFS `.txt` files were processed using pandas in a Jupyter notebook.

### Steps Involved

1. **Merged `stop_times.txt` with `stops.txt`** to get latitude/longitude and timestamps.
2. **Sorted by `trip_id` and `stop_sequence`** to ensure sequential travel.
3. **Calculated `travel_time_sec`** between consecutive stops.
4. **Extracted time features**: hour of day, service type (`weekday`, `saturday`, etc.).
5. **Merged with `trips.txt`** to get `route_id`, `direction_id`, and shape ID.
6. **Handled anomalies** like missing values and duplicated trips.
7. **Saved the final DataFrame** as `gtfs_cleaned.csv`.

### Main Cleaned File: `gtfs_cleaned.csv`

| Feature | Description |
|---|---|
| `trip_id` | Unique trip reference |
| `stop_sequence` | Order of stop in trip |
| `arrival_time / dep.` | Timestamps from `stop_times.txt` |
| `stop_lat / stop_lon` | From `stops.txt` |
| `travel_time_sec` | Time between current and next stop |
| `direction_id` | Direction (0 or 1) |
| `service_id` | Weekday/weekend/saturday |
| `hour` | Extracted from time for patterning |
| `shape_dist_traveled` | Distance covered on shape route |

### Libraries Used (Preprocessing)

```python
import pandas as pd
from datetime import timedelta
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error
import matplotlib.pyplot as plt
```

---

## Streamlit Dashboard

**Left Panel:** Trip planner (source, destination, transport mode, traffic toggle) and LLM travel plan.

**Right Panel:** Interactive map showing the selected route.

An interactive UI for users to:

- Select source and destination.
- Predict travel time using the trained ML model and LLM advisory.
- View routes on **Folium (OpenStreetMap)**.
- Display signal points and their status.
- Select a mode of transport.

### Dashboard Features

| Feature | Description |
|---|---|
| **Folium Map** | OpenStreetMap centered on Delhi |
| **Stop Info** | Hover to view arrival/departure time |
| **Prediction Box** | Select source → destination → plan |
| **TomTom Routing API** | Provides routes, travel time, distance, and traffic delays |
| **OpenWeather API** | Fetches temperature and weather conditions for the journey |
| **LLM (Groq) Insights** | Generates a natural language summary with travel time, distance, expected congestion hotspots, weather impact, and suggestions |

### Libraries Used (Dashboard)

```python
import streamlit as st
import folium
from streamlit_folium import st_folium
import requests
import pandas as pd
from dotenv import load_dotenv

from services.tomtom_service import geocode_tomtom, route_tomtom
from services.weather_service import weather_onecall
from services.llm_service import ask_llm
```

API providers:

- [TomTom Developer Portal](https://developer.tomtom.com/)
- [OpenWeather](https://openweathermap.org/)
- [Groq Console](https://console.groq.com/)

---

## Installation Setup

### 1. Clone the repository

```bash
git clone https://github.com/Harshit-0021/Smartroute-delhi.git
cd Smartroute-delhi
```

### 2. Create a virtual environment (recommended)

```bash
python -m venv .venv
```

On Windows:

```powershell
.\.venv\Scripts\Activate.ps1
```

On macOS/Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install -r requirements.txt
```

### 4. Configure API keys

Create a `secrets.env` file in the project root:

```dotenv
TOMTOM_API_KEY=your_tomtom_api_key
OPENWEATHER_API_KEY=your_openweather_api_key
GROQ_API_KEY=your_groq_api_key
```

Replace the placeholders with your own API keys. Do not upload `secrets.env` to GitHub.

### 5. Run the application

```bash
python -m streamlit run dashboard.py
```

---

## Project Structure

```text
Smartroute-delhi/
│── dashboard.py                 # Main Streamlit application
│── Dataset/                     # Dataset files used by the project
│── services/                    # API service integrations
│   ├── tomtom_service.py         # TomTom geocoding and routing
│   ├── weather_service.py       # OpenWeather API integration
│   └── llm_service.py           # Groq LLM integration
│── .gitignore                   # Ignored files for Git
│── requirements.txt             # Python dependencies
│── README.md                    # Project documentation
```

---

## Author

Harshit

GitHub: [Harshit-0021](https://github.com/Harshit-0021)

Project Repository: [Smartroute-delhi](https://github.com/Harshit-0021/Smartroute-delhi)

---

Built with care — let's build and make a change!
