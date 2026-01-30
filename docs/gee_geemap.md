# Google Earth Engine & Geemap

Google Earth Engine (GEE) provides cloud-based access to the world's satellite data (Sentinel, Landsat, MODIS). `geemap` is the Python library that enables interactive mapping in Jupyter environments.

## 🚀 Interactive Mapping

The `Map` object allows you to visualize GEE assets directly in your notebook.

```python
import ee
import geemap

# Initialize the map
Map = geemap.Map(center=[20, 85], zoom=6)

# Add a Sentinel-2 Image Collection
s2 = ee.ImageCollection("COPERNICUS/S2_SR_HARMONIZED") \
    .filterBounds(Map.getBounds()) \
    .filterDate('2023-01-01', '2023-12-31') \
    .median()

# Visualization parameters
vis_params = {
    'min': 0,
    'max': 3000,
    'bands': ['B4', 'B3', 'B2'] # True Color
}

Map.addLayer(s2, vis_params, 'Sentinel-2 2023')
Map
```

---

## 🔁 Comparing JavaScript & Python

While the logic is the same, the syntax varies slightly between the GEE Code Editor (JS) and Python.

| Feature | GEE JavaScript (Client) | Geemap Python |
| :--- | :--- | :--- |
| **Object Print** | `print(obj)` | `geemap.ee_print(obj)` |
| **Layer Addition** | `Map.addLayer(obj)` | `Map.addLayer(obj)` |
| **UI Widgets** | `ui.Button()`, etc. | `ipywidgets` (Native) |
| **Export** | `Export.image.toDrive()` | `geemap.ee_export_image()` |

---

## 🛠️ Geemap Utility Features

`geemap` adds many "helper" functions that don't exist in the base GEE API:

- **Converter**: Convert GEE JS to Python automatically (`geemap.js_to_python`).
- **Data download**: Download GEE pixels directly to local NumPy arrays.
- **Charts**: Create time-series charts with one line of code.

!!! success "Cloud Power"
    With GEE, you can process global-scale datasets without downloading a single byte to your computer.
