# GEE & Geemap

Google Earth Engine (GEE) is a multi-petabyte catalog of satellite imagery and geospatial datasets. `geemap` is the bridge between GEE and the Python ecosystem.

## 🚀 Quick Start with Geemap

First, install the library:

```bash
pip install geemap
```

### Creating an Interactive Map

```python
import ee
import geemap

# Initialize the map
Map = geemap.Map()

# Add Earth Engine data
image = ee.Image('USGS/SRTMGL1_003')
Map.addLayer(image, {'min': 0, 'max': 3000}, 'SRTM DEM')

Map
```

## 📊 Key Features

* Convert GEE JavaScripts to Python.
* Interactive editing of GEE data.
* Exporting GEE images and tables directly to your local drive.
