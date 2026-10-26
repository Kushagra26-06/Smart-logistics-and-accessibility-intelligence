# Smart Logistics & Accessibility Intelligence Platform

A comprehensive AI-powered route monitoring and risk assessment system for North East India (NER) logistics operations.

## Overview

This platform provides real-time intelligence for logistics operations in the challenging terrain of North East India. It combines simulated weather data, field incident reports, and historical disruption records to calculate optimal routes, predict disruptions, and ensure safe transportation.

## Technical Approach

The system follows a **data-in → intelligence → action-out** pipeline. Instead of displaying static information, it continuously ingests live and field data, processes it through an AI risk model, and acts on the results (rerouting, alerting).

### Core Methodology (3 Stages)

#### Stage 1 — Data Collection
Continuously gather inputs from three sources:
- **Simulated Weather Data** (temperature, humidity, wind, precipitation, pressure)
- **Field officer reports** (manually entered GPS coordinates, incident details)
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
- **Simulated Weather Data** (for prototype demonstration)
- **Field incident reports** (submitted via web form with manual GPS entry)
- **Historical/seeded route disruption data**

### Layer 2: Processing Layer (The "Brain")
- **Risk Scoring Engine** — runs weighted formula on incoming data
- **Dijkstra Route Optimizer** — computes optimal paths using current risk-weighted graph

### Layer 3: Storage Layer
- **localStorage** — stores: incident reports, route risk scores, historical logs
- **Future**: Firebase/MongoDB for production deployment

### Layer 4: Presentation Layer (What Users See)
- **Dashboard** — live GIS map + AI Route Intelligence panel
- **Route Analytics** — historical trends and reliability rankings
- **Weather Monitoring** — live district weather cards
- **Incident Reports** — field report feed and submission form

## Features

### 1. Interactive Route Map
- Live GIS map of NER districts with route visualization using Google Maps
- Color-coded routes based on current risk levels (Green: Clear, Yellow: At Risk, Red: Blocked)
- Scroll-to-zoom functionality (no Ctrl key required)
- Immediate route rendering on page load (no blank map)
- Click on routes to view detailed information and AI analysis

### 2. AI-Powered Route Intelligence
- **Optimal Route Finder**: Select start/end districts from dropdown, get the safest + shortest path using Dijkstra's algorithm
- **Disruption Simulation**: Simulate risk spikes to see alternative routes
- **Dynamic Route Optimization**: Routes update based on real-time conditions
- **Weighted Decision Making**: Balances distance, risk, and congestion
- **Visual Path Display**: Optimal paths shown with distinctive dashed red line with start/end markers

### 3. Route Analytics Tab
- **Reliability Rankings Table**: Historical uptime percentage for all routes with trend indicators
- **Disruption Leaderboard**: Routes with most incidents in last 30 days, ranked by frequency
- **Risk Trend Analysis**: Interactive 30-day risk score visualization using Chart.js
- **Pattern Recognition**: Identify seasonal and weather-based risk patterns
- **Route Selector**: Choose specific routes to view their risk trends over time

### 4. Weather Monitoring Tab
- **5 Key Metrics** with route risk relevance:
  - **Temperature** (road surface risk indicator) - shows current temp with emoji icon and risk explanation
  - **Humidity** (pre-monsoon storm indicator) - predicts storm buildup before rainfall
  - **Wind Speed & Direction** (storm movement tracker) - animated wind arrow showing direction
  - **Precipitation** (#1 Risk Driver - landslide/flood correlation) - color-coded status (Green/Yellow/Red)
  - **Barometric Pressure** (predictive storm indicator) - shows trend and warning alerts
- **District Weather Grid**: Real-time conditions across all 10 NER districts with color-coded risk levels
- **Predictive Warnings**: Storm alerts based on pressure trends (pressure dropping rapidly)
- **Auto-refresh**: Weather data updates every 30 seconds

### 5. Incident Reports Tab
- **Field Report Submission**: Manual GPS coordinate entry for incident location
- **Incident Types**: Landslide, Flood, Road Damage, Traffic Congestion, Bridge Issues
- **Severity Levels**: Low, Medium, High, Critical with color-coded badges
- **Report List View**: Shows all submitted reports with type, location, date, severity, and description preview
- **Report Detail View**: Click any report to view full details including all fields
- **Delete Functionality**: Delete reports from both list view and detail view with 🗑️ icon/button
- **Persistent Storage**: Reports saved to localStorage database
- **No Location Permission Prompts**: Manual entry only, no automatic GPS requests

### 6. Dashboard Tab
- **Route Status Overview**: Real-time counts of Clear, At Risk, and Blocked routes
- **Weather Conditions Panel**: Live temperature, humidity, wind speed, and rainfall data
- **Alerts & Notifications**: High-risk alerts based on weather conditions (e.g., heavy rainfall warnings)
- **Route Status List**: Clickable list of all routes with their current status and risk scores
- **Optimal Route Finder Form**: Integrated route finding tool under the map
- **Field Incident Report Form**: Submit new incidents directly from dashboard

### 7. User Interface
- **Dark Mode Only**: Optimized dark theme for better visibility and reduced eye strain
- **Responsive Design**: Works on desktop and tablet screens
- **Tab Navigation**: Easy switching between Dashboard, Route Analytics, Weather Monitoring, and Incident Reports
- **Real-time Updates**: Weather data refreshes every 30 seconds, route status updates every 60 seconds
- **Clean, Modern Design**: Professional gradient styling with intuitive color coding

## Setup Instructions

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Google Maps API Key (required for map functionality)
- No server required — runs entirely in the browser

### Installation
1. Clone or download the project files
2. Ensure you have a valid Google Maps API key
3. Open `index.html` in a web browser
4. The application will load immediately with the map and all features

### Google Maps API Key
The project uses Google Maps for route visualization. A valid API key is required and is included in the code. To use your own API key:
1. Get a Google Maps JavaScript API key from [Google Cloud Console](https://console.cloud.google.com/)
2. Replace the existing API key in the code (line in the script tag)
3. Ensure the API key has Maps JavaScript API enabled

### First-Time Use
1. **Dashboard**: View the interactive map with NER districts and routes. All routes load immediately on startup.
2. **Route Intelligence**:
   - Select start and end districts from the dropdowns under the map
   - Click "🔍 Find Optimal Route" to see the best path displayed on the map
   - View results showing total distance, risk score, estimated time, and path
   - Click "⚡ Simulate Disruption" to see alternative routes under high-risk conditions
3. **Route Analytics Tab**: Click the tab to view:
   - Reliability rankings table with historical uptime percentages
   - Disruption leaderboard showing most problematic routes
   - Interactive risk trend charts for selected routes
4. **Weather Monitoring Tab**: Click the tab to view:
   - Central monitoring station metrics (Guwahati)
   - District weather grid with color-coded risk levels
   - Predictive warnings based on pressure trends
5. **Incident Reports Tab**: Click the tab to:
   - View all submitted incident reports in a list
   - Click any report to view full details
   - Delete reports using the 🗑️ icon (list view) or "Delete Report" button (detail view)
6. **Submit Incident Report**:
   - Use the form on the Dashboard tab
   - Select incident type, enter GPS coordinates manually, describe the incident, and set severity
   - Click submit to save the report (no success alert, just saves to localStorage)

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
- Precipitation > 30mm/hr: +50 to risk score
- Precipitation 10-30mm/hr: +25 to risk score
- Humidity > 80% + rising temp: +15 to risk score
- Pressure dropping > 3hPa/3hr: +20 to risk score
- Wind speed > 30km/h: +10 to risk score

## Technology Stack

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Mapping**: Google Maps JavaScript API for interactive GIS maps
- **Charts**: Chart.js for data visualization
- **Storage**: localStorage for prototype data persistence
- **Algorithm**: Dijkstra's shortest path algorithm for route optimization
- **Weather**: Simulated weather data (ready for OpenWeatherMap API integration)

## Tab-by-Tab Feature Guide

### Dashboard Tab
The main interface featuring:
- **Interactive Map**: Google Maps showing all NER routes with color-coded risk levels
- **Route Status Cards**: Three cards showing real-time counts of Clear, At Risk, and Blocked routes
- **Weather Panel**: Live weather conditions for the NER region
- **Alerts Box**: High-risk alerts based on weather conditions
- **Route List**: Clickable list of all routes with status badges
- **Optimal Route Finder**: Tool to find the best path between any two districts
- **Incident Report Form**: Submit new field reports

### Route Analytics Tab
Historical analysis and trends:
- **Reliability Table**: Shows route reliability rankings with percentage uptime and trend indicators (Improving/Declining/Stable)
- **Disruption Leaderboard**: Top 5 most disrupted routes with incident counts
- **Risk Trend Chart**: Interactive line chart showing 30-day risk history for selected routes
- **Route Selector**: Dropdown to choose which route's trend to visualize

### Weather Monitoring Tab
Real-time weather intelligence:
- **Central Station Metrics**: Detailed breakdown of 5 key weather metrics with risk explanations
- **District Weather Grid**: Cards for each of 10 districts showing current conditions
- **Color-Coded Risk**: Green (low risk), Yellow (medium risk), Red (high risk) based on precipitation
- **Predictive Alerts**: Pressure trend warnings for approaching storms

### Incident Reports Tab
Field report management:
- **Report List**: All submitted reports with type, location, date, severity, and description preview
- **Detail View**: Full report details when clicking on a report
- **Delete Function**: Remove reports with 🗑️ icon or "Delete Report" button
- **Persistent Storage**: All reports saved to browser localStorage

## Key Improvements from Initial Version

1. **Google Maps Integration**: Replaced Leaflet/OpenStreetMap with Google Maps for better reliability and performance
2. **Dark Mode Only**: Removed light mode for a consistent, optimized dark theme
3. **Manual GPS Entry**: Removed automatic location requests to eliminate permission prompts
4. **Delete Functionality**: Added ability to delete incident reports
5. **Fixed Data Display**: Corrected incident report field display issues
6. **Improved Map UX**: Enabled scroll-to-zoom without Ctrl key, fixed blank map on startup
7. **Removed Settings Tab**: Simplified navigation by removing theme settings

## Future Enhancements

### Planned Features
- **Real-time Weather API Integration**: Connect to OpenWeatherMap for live weather data
- **Firebase Integration**: Cloud database for multi-user support and data persistence
- **Mobile App**: React Native mobile application for field officers
- **Push Notifications**: Real-time alerts for route disruptions to mobile devices
- **ML Model**: Train on historical data for predictive analytics
- **User Authentication**: Role-based access (admin, field officer, logistics manager)
- **Photo Upload**: Attach photos to incident reports
- **GPS Auto-Capture**: Optional automatic GPS location capture with user permission

### Scalability
- Add more NER districts and routes
- Integrate with national highway databases
- Connect to state transportation APIs
- Multi-region support (beyond NER)
- Real-time data synchronization across devices

## License

This project is a prototype for demonstration purposes.

## Contact

For questions or feedback about this platform, please contact the development team.

---

**Version**: 1.1.0 (Prototype with Google Maps)
**Last Updated**: October 2026
**Status**: Development Prototype
