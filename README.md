# Texas State University Parking Optimization System
hi this is raghav

## Research Questions

1. Which campus site provides the optimal location for a new parking facility to maximize benefits for dorm residents and the general student body?
2. How can we develop an algorithm to match incoming students with vehicles to the best dorm locations, considering factors like parking proximity, available spaces, and individual preferences?

## Data Sources

### Core Datasets

1. **students.csv**
   - Columns: Campus, Class, Department, Gender, Housing, Level, Semester, Year
   - Contains enrollment data by college, major, and housing
   - Year stored as float64, Semester as categorical (Fall, Spring, Summer)

2. **dorm.csv**
   - Columns: dorm_name, capacity, latitude, longitude, formatted_address, place_id
   - Contains dorm information and geolocation data

3. **green_lots.csv**
   - Columns: Location, Latitude, Longitude, Spaces
   - Contains parking lot information and capacity data

### Data Collection Process

- Used Firecrawl API to scrape parking statistics
- Utilized Google Maps API for geocoding dorm locations
- Manually corrected some coordinates for improved accuracy
- Performed inner joins on name columns for data consolidation

## Methodology

### 1. Data Preprocessing

- Filter students.csv to latest 2 semesters (2024)
- Calculate student density per dorm
- Segment university into clusters based on student population distribution

### 2. Desirability Score Calculation

The desirability score combines three main components:

1. **Dorm Proximity**
   - Calculated using minimum distance to dorms
   - Higher scores for closer locations

2. **Resident Density**
   - Measures student population within 100m radius
   - Considers dorm capacity in calculations

3. **Existing Parking Supply**
   - Acts as a penalty factor
   - Higher penalty for areas with many nearby lots
   - No penalty for dorms without nearby parking

Formula:
```
desirability = (proximity_score * w1) + (density_score * w2) - (parking_penalty * w3)
```

### 3. Machine Learning Model

Using KNN Clustering to predict desirability scores:

1. **Features**
   - Candidate location coordinates (lat/long)
   - Distance to nearest dorm
   - Local resident density
   - Nearby parking capacity

2. **Training Process**
   - Split data into training/testing sets
   - Normalize features
   - Train model to predict desirability scores
   - Plot loss curves to monitor overfitting

### 4. Visualization

- Using OpenStreetMap for base map
- Saving as HTML file for Google Colab compatibility
- Map elements:
  - Dorm locations (house icons)
  - Existing parking (car icons)
  - Top 3 recommended locations
  - Interactive tooltips with location details

## Current Implementation

### Data Structure
```python
{
    'dorm_name': str,          # Name of dorm
    'total_beds': int,         # Total bed capacity
    'total_parking': int,      # Total allocated parking spaces
    'used_parking': int,       # Counter for used parking spaces
    'occupied_beds': int       # Counter for occupied beds
}
```

### Parking Space Allocation Examples

1. Matthews Street Garage (628 spaces)
   - Jackson: 257 spaces
   - Gallardia: 192 spaces
   - CTQ: 192 spaces

2. Edward Gary Street Garage (285 spaces)
   - Sterry: 118 spaces
   - Lantana: 76 spaces
   - Butler: 91 spaces

3. San Jacinto Garage (227 spaces)
   - San Jacinto: 121 spaces
   - San Marcos: 106 spaces

### Assignment Logic

```python
if student needs parking:
    check (bed availability < 95% AND parking available)
else:
    check (bed availability < 95% only)
```

## Future Improvements

1. Refined clustering algorithms for population density
2. Enhanced penalty system for parking proximity
3. Integration of additional student preference factors
4. Dynamic weight adjustment based on seasonal patterns

## Usage

[Add usage instructions when implementation is complete]

## Dependencies

- Python 3.x
- Pandas
- NumPy
- Scikit-learn
- Folium (for visualization)
- PapaParse (for CSV processing)
- Lodash (for data manipulation)
