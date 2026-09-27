# Smart Logistics & Accessibility Intelligence Platform

A comprehensive AI-powered route monitoring and risk assessment system for North East India (NER) logistics operations.

## Overview

This platform provides real-time intelligence for logistics operations in the challenging terrain of North East India. It combines live weather data, field incident reports, and historical disruption records to calculate optimal routes, predict disruptions, and ensure safe transportation.

## Technical Approach

The system follows a **data-in → intelligence → action-out** pipeline. Instead of displaying static information, it continuously ingests live and field data, processes it through an AI risk model, and acts on the results (rerouting, alerting).

### Core Methodology (3 Stages)

#### Stage 1 — Data Collection
Continuously gather live inputs from three sources:
- **Weather API** (temperature, humidity, wind, precipitation, pressure)
- **Field officer reports** (geo-tagged incidents, photos)
- **Historical/seeded disruption records**

#### Stage 2 — Intelligence Processing
A weighted risk-scoring formula combines these inputs into a single risk score (0–100) per route segment:
- Precipitation and pressure trend are weighted highest (strongest predictors of landslides/floods in NER terrain)
- Dijkstra's algorithm runs on a graph where roads are edges weighted by this risk score (not just distance)
- Finds the mathematically optimal (safest + shortest) path between any two districts

#### Stage 3 — Action & Visualization
Results are displayed on a GIS dashboard with:
- Color-coded routes (green/yellow/red based on risk)
- Automatic alternate route suggestions when risk crosses thresholds
- Real-time alerts for high-risk conditions

### Why This Approach

**Transparent, Explainable Model**
- Chose a rule-based risk model over black-box ML
- No years of NER-specific training data available
- Weighted formula is easier to justify, debug, and trust

**Dijkstra Algorithm**
- Guarantees mathematically optimal path for graphs with non-negative weights
- Fast enough to run in real-time for NER's size (15–25 major nodes)
- No need for heavier, more complex algorithms

## Architecture

The system is structured in 4 layers, each with clear responsibilities:

### Layer 1: Data Layer (Inputs)
- **Weather API** (OpenWeatherMap integration)
- **Field incident reports** (submitted via web form)
- **Historical/seeded route disruption data**

### Layer 2: Processing Layer (The "Brain")
- **Risk Scoring Engine** — runs weighted formula on incoming data
- **Dijkstra Route Optimizer** — computes optimal paths using current risk-weighted graph

### Layer 3: Storage Layer
- **localStorage** (prototype) — stores: live weather readings, route risk scores, incident reports, historical logs
- **Future**: Firebase/MongoDB for production deployment

### Layer 4: Presentation Layer (What Users See)
- **Dashboard** — live GIS map + AI Route Intelligence panel
- **Route Analytics** — historical trends and reliability rankings
- **Weather Monitoring** — live district weather cards
- **Incident Reports** — field report feed and submission form
- **Settings** — user preferences, language, role switching

## Features

### 1. Interactive Route Map
- Live GIS map of NER districts with route visualization
- Color-coded routes based on current risk levels
- Real-time weather overlay
- District markers with connectivity visualization

### 2. AI-Powered Route Intelligence
- **Optimal Route Finder**: Select start/end districts, get the safest + shortest path
- **Disruption Simulation**: Simulate risk spikes to see alternative routes
- **Dynamic Route Optimization**: Routes update based on real-time conditions
- **Weighted Decision Making**: Balances distance, risk, and congestion

### 3. Route Analytics
- **Reliability Rankings**: Historical uptime percentage for all routes
- **Disruption Leaderboard**: Routes with most incidents in last 30 days
- **Risk Trend Analysis**: 30-day risk score visualization with Chart.js
- **Pattern Recognition**: Identify seasonal and weather-based risk patterns

### 4. Weather Monitoring
- **5 Key Metrics** with route risk relevance:
  - Temperature (road surface risk indicator)
  - Humidity (pre-monsoon storm indicator)
  - Wind Speed & Direction (storm movement tracker)
  - Precipitation (#1 Risk Driver - landslide/flood correlation)
  - Barometric Pressure (predictive storm indicator)
- **District Weather Grid**: Real-time conditions across all 10 NER districts
- **Color-coded Risk Levels**: Green (normal), Yellow (elevated), Red (high)
- **Predictive Warnings**: Storm alerts based on pressure trends

### 5. Incident Reports
- **Field Report Submission**: Geo-tagged incident reporting
- **Incident Types**: Landslide, Flood, Road Damage, Traffic Congestion, Bridge Issues
- **Severity Levels**: Low, Medium, High, Critical
- **Report Management**: View, delete, and manage all submitted reports
- **Persistent Storage**: Reports saved to localStorage database

### 6. User Interface
- **Dark/Light Mode**: Theme switching with localStorage persistence
- **Responsive Design**: Works on desktop and tablet screens
- **Tab Navigation**: Easy switching between Dashboard, Route Analytics, Weather Monitoring, and Incident Reports
- **Real-time Updates**: Weather data refreshes every 30 seconds

## Setup Instructions

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- No server required — runs entirely in the browser

### Installation
1. Clone or download the project files
2. Open `index.html` in a web browser
3. The application will load immediately

### First-Time Use
1. **Dashboard**: View the interactive map with NER districts and routes
2. **Route Intelligence**: 
   - Select start and end districts from dropdowns
   - Click "🔍 Find Optimal Route" to see the best path
   - Click "⚡ Simulate Disruption" to see alternative routes
3. **Route Analytics**: Click the tab to view historical reliability and disruption data
4. **Weather Monitoring**: Click the tab to see real-time weather conditions
5. **Incident Reports**: 
   - Submit a new report using the form on Dashboard
   - Click "Incident Reports" tab to view all submitted reports
   - Click any report to view full details
   - Delete reports using the delete button

## NER Districts Covered

The system covers 10 major districts in North East India:
- Guwahati (Central Monitoring Station)
- Shillong
- Imphal
- Kohima
- Aizawl
- Agartala
- Itanagar
- Gangtok
- Dibrugarh
- Silchar

## Risk Scoring Formula

The risk score (0–100) for each route segment is calculated using:

```
Risk Score = (Distance × 0.3) + (Risk × 0.4) + (Congestion × 0.3)
```

Where:
- **Distance**: Physical road distance in km
- **Risk**: Terrain difficulty + weather impact + historical incidents
- **Congestion**: Current traffic conditions

### Weather Impact on Risk
 precipitation > 30mm/hr: +50 to risk score
 precipitation 10-30mm/hr: +25 to risk score
 Humidity > 80% + rising temp: +15 to risk score
 Pressure dropping > 3hPa/3hr: +20 to risk score
 Wind speed > 30km/h: +10 to risk score

## Technology Stack

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Mapping**: Leaflet.js for interactive GIS maps
- **Charts**: Chart.js for data visualization
- **Storage**: localStorage (prototype), Firebase/MongoDB (production)
- **Weather**: OpenWeatherMap API (ready for integration)
- **Algorithm**: Dijkstra's shortest path algorithm

## Future Enhancements

### Planned Features
- **Real-time Weather API Integration**: Connect to OpenWeatherMap for live data
- **Firebase Integration**: Cloud database for multi-user support
- **Mobile App**: React Native mobile application
- **Push Notifications**: Real-time alerts for route disruptions
- **ML Model**: Train on historical data for predictive analytics
- **User Authentication**: Role-based access (admin, field officer, logistics manager)
- **Photo Upload**: Attach photos to incident reports
- **GPS Integration**: Auto-capture location for incident reports

### Scalability
- Add more NER districts and routes
- Integrate with national highway databases
- Connect to state transportation APIs
- Multi-region support (beyond NER)

## License

This project is a prototype for demonstration purposes.

## Contact

For questions or feedback about this platform, please contact the development team.

---

**Version**: 1.0.0 (Prototype)
**Last Updated**: September 2026
**Status**: Development Prototype
