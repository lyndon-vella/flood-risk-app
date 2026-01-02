# Flood Risk App Specification

## Overview
A web app that shows flood risk for properties based on sea level rise projections.

## Requirements
- Search for an address
- Display property on 3D map with terrain
- Simulate sea level rise (slider 0-10ft)
- Show flood risk assessment
- Provide actionable advice

## Tech Stack
- **Framework**: SvelteKit
- **3D Map**: CesiumJS
- **Styling**: CSS (minimal, no framework)
- **Geocoding**: Nominatim (free)
- **Terrain**: Cesium World Terrain

## Design Guidelines
- Clean, minimal UI
- Address search bar at top
- Full-screen 3D map
- Floating control panel for slider
- Side panel for property info/risk report
- Color coding: green (safe) → yellow → orange → red (high risk)

## Milestones

### Milestone 1: UI Setup
- [ ] Create SvelteKit project
- [ ] Set up basic layout (header, map area, side panel)
- [ ] Add address search input
- [ ] Add sea level slider component
- [ ] Style with minimal CSS

### Milestone 2: Cesium Integration
- [ ] Install CesiumJS
- [ ] Create Cesium viewer component
- [ ] Load 3D terrain
- [ ] Add OSM buildings layer

### Milestone 3: Address Search
- [ ] Integrate Nominatim geocoding API
- [ ] Fly camera to searched location
- [ ] Display location marker

### Milestone 4: Flood Simulation
- [ ] Create water surface entity
- [ ] Connect slider to water level
- [ ] Animate water rise/fall

### Milestone 5: Risk Assessment
- [ ] Calculate property elevation
- [ ] Determine flood threshold
- [ ] Generate risk report
- [ ] Display advice panel

### Milestone 6: Polish
- [ ] Loading states
- [ ] Error handling
- [ ] Mobile responsive
- [ ] Deploy to Vercel
