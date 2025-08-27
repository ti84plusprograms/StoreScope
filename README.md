# StoreScope

A storefront visibility analysis tool that calculates how visible a business location is from the nearest road using computer vision and geospatial analysis.

## Overview

StoreScope helps businesses and urban planners assess the visibility of storefronts from nearby roads by analyzing multiple factors including distance, viewing angle, and visual obstructions. The tool combines Google Maps APIs with Google Cloud Vision AI to provide comprehensive visibility scoring.

## Features

- **Address Geocoding**: Convert street addresses to precise latitude/longitude coordinates
- **Nearest Road Detection**: Find the closest road to any given location
- **Distance Calculation**: Compute accurate distances using the Haversine formula
- **Bearing Analysis**: Calculate viewing angles from roads to storefronts
- **Street View Integration**: Automatically fetch Google Street View images
- **Object Detection**: Identify visual obstructions using Google Cloud Vision AI
- **Visibility Scoring**: Generate comprehensive visibility scores (0-1 scale)
- **Traffic Data Analysis**: Analyze traffic patterns using included datasets

## Installation

### Prerequisites

- Python 3.8 or higher
- Google Cloud Platform account with enabled APIs
- Google Maps API key

### Required APIs

Enable the following Google Cloud APIs:
- Google Maps Geocoding API
- Google Roads API
- Google Street View Static API
- Google Cloud Vision API

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ti84plusprograms/StoreScope.git
   cd StoreScope
   ```

2. **Install dependencies:**
   ```bash
   pip install -r req.txt
   ```

3. **Set up environment variables:**
   Create a `.env` file in the project root:
   ```env
   GOOGLE_API_KEY=your_google_maps_api_key_here
   ```

4. **Configure Google Cloud credentials:**
   ```bash
   export GOOGLE_APPLICATION_CREDENTIALS="path/to/your/service-account-key.json"
   ```

## Usage

### Basic Usage

Run the main script and enter an address when prompted:

```bash
python initial.py
```

The program will:
1. Geocode the provided address
2. Find the nearest road
3. Calculate distance and bearing
4. Fetch a Street View image
5. Analyze visual obstructions
6. Generate a visibility score

### Example Output

```
Enter the address: 123 Main Street, Anytown, USA
Latitude: 40.7128, Longitude: -74.0060
Nearest Road Latitude: 40.7130, Nearest Road Longitude: -74.0058
Distance from 123 Main Street, Anytown, USA to nearest road: 0.025 km
Angle from 123 Main Street, Anytown, USA to nearest road: 45.2°
Street view image saved as street_view_image.jpg
Object: Car, Confidence: 0.92
Object: Tree, Confidence: 0.87
Blocked Object: Car, Area: 0.05
Total area blocked by objects: 12.5%
Visibility Score: 0.73
```

### Jupyter Notebooks

For interactive analysis, use the provided Jupyter notebooks:

- `initial.ipynb`: Main analysis workflow
- `traffic_data.ipynb`: Traffic pattern analysis

```bash
jupyter notebook initial.ipynb
```

## Algorithm

The visibility score is calculated using three key factors:

### 1. Distance Factor
Uses exponential decay to account for viewing distance:
```python
distance_factor = math.exp(-alpha * distance)
```

### 2. Angle Factor  
Uses cosine to account for viewing angle optimality:
```python
angle_factor = math.cos(angle_radians)
```

### 3. Obstruction Factor
Accounts for visual obstructions detected in Street View:
```python
obstruction_factor = 1 - (obstruction_percent / 100)
```

### Final Score
```python
visibility_score = distance_factor * angle_factor * obstruction_factor
```

The final score is normalized to a 0-1 scale, where:
- **1.0**: Perfect visibility (close, direct view, no obstructions)
- **0.5**: Moderate visibility 
- **0.0**: No visibility (too far, poor angle, or completely obstructed)

## File Structure

```
StoreScope/
├── README.md                    # This file
├── initial.py                   # Main analysis script
├── initial.ipynb               # Interactive Jupyter notebook
├── traffic_data.ipynb          # Traffic analysis notebook  
├── req.txt                     # Python dependencies
├── cleaned_data/               # Traffic datasets
│   └── traffic_data_sample_filtered.parquet
└── Storescope Hacklytics 2025 Technical Write Up.pdf
```

## Dependencies

- **google-cloud-vision**: Object detection and image analysis
- **google-api-core**: Google API client libraries
- **requests**: HTTP requests for API calls
- **pillow**: Image processing
- **python-dotenv**: Environment variable management
- **geopy**: Geospatial calculations
- **pyarrow**: Parquet data file handling

## API Requirements

### Google Maps APIs
- **Geocoding API**: Convert addresses to coordinates
- **Roads API**: Find nearest roads
- **Street View Static API**: Fetch street view images

### Google Cloud Vision API
- **Object Localization**: Detect and locate objects in images

## Traffic Data Analysis

The project includes traffic data analysis capabilities using OpenStreetMap data. The `traffic_data.ipynb` notebook provides tools for:

- Loading and filtering traffic datasets
- Analyzing traffic patterns by road type
- Correlating traffic volume with visibility factors

## Contributing

This project was developed for Hacklytics 2025. Contributions are welcome for:

- Algorithm improvements
- Additional data sources
- Performance optimizations
- New analysis features

## License

This project is part of the Hacklytics 2025 hackathon submission.

## Troubleshooting

### Common Issues

1. **API Key Errors**: Ensure your Google API key is valid and has the required APIs enabled
2. **Authentication Errors**: Verify your Google Cloud service account credentials are properly configured
3. **Import Errors**: Install all dependencies using `pip install -r req.txt`
4. **Rate Limiting**: Google APIs have usage limits; implement delays if needed

### Support

For technical questions or issues, please refer to the technical write-up document included in the repository.
