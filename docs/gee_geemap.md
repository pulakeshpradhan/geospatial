# Google Earth Engine & Geemap: The Cloud Frontier

Google Earth Engine (GEE) represents a fundamental shift in geospatial analysis. Moving from "data-download" to "code-to-data," it provides a multi-petabyte catalog of satellite imagery and the computational power to process it across thousands of cores. This guide explores the architecture of the **GEE ecosystem** and how to master it via Python using `geemap`.

---

## 🏛️ Section 1: The GEE Architecture (Client vs. Server)

One of the steepest learning curves in GEE is understanding why a standard Python `for` loop or `if` statement doesn't work on Earth Engine objects.

### 1.1 Client-Side vs. Server-Side

* **Client-Side (Python/JS)**: This is your browser or your Jupyter notebook. It handles the interface and instructions.
* **Server-Side (Google)**: This is where the actual images are stored.
* **The Bridge**: When you write `image.add(1)`, you aren't adding one to a number in RAM. You are sending a **Instruction (JSON)** to Google saying: "Find this image in your database and add one to every pixel."

### 1.2 Deferred Execution (Lazy Loading)

Nothing actually happens until you request a result (e.g., `Map.addLayer`, `getInfo()`, or `Export`). This allows GEE to only process the specific pixels visible on your screen at a specific zoom level.

---

## 🛰️ Section 2: Mastering ImageCollections & Filtering

An `ImageCollection` is a stack of images. Learning to filter this stack efficiently is the key to global analysis.

=== "Advanced Filtering"

    ```python
    import ee
    import geemap

    # Define a point of interest
    roi = ee.Geometry.Point([85.8, 20.3])

    # Load Sentinel-2 Collection (Bottom of Atmosphere)
    s2_collection = ee.ImageCollection("COPERNICUS/S2_SR_HARMONIZED") \
        .filterBounds(roi) \
        .filterDate('2023-01-01', '2023-06-30') \
        .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 10)) \
        .sort('CLOUDY_PIXEL_PERCENTAGE')

    print(f"Number of clear images found: {s2_collection.size().getInfo()}")
    ```

=== "Reducers: Turning Pixels into Stats"
    Reducers allow you to aggregate data across space or time.

    ```python
    # Temporal Reducer: Create a Median Composite (Cloud-free look)
    median_image = s2_collection.median()

    # Spatial Reducer: Calculate mean temperature for a district
    temp_collection = ee.ImageCollection("MODIS/061/MOD11A1")
    mean_temp = temp_collection.mean().reduceRegion(
        reducer=ee.Reducer.mean(),
        geometry=roi,
        scale=1000
    )
    ```

---

## 🌍 Section 3: Time-Series Analysis & Change Detection

One of GEE's greatest strengths is analyzing changes over decades.

### 3.1 Map and Iterate

In Python, we use `.map()` to apply a function to every image in a collection.

```python
def calculate_indices(image):
    # Calculate NDVI and NDWI simultaneously
    ndvi = image.normalizedDifference(['B8', 'B4']).rename('NDVI')
    ndwi = image.normalizedDifference(['B3', 'B8']).rename('NDWI')
    return image.addBands([ndvi, ndwi])

# Apply to the whole collection
processed_collection = s2_collection.map(calculate_indices)
```

### 3.2 Time-Series Plotting with Geemap

`geemap` makes it incredibly easy to create interactive charts that would require hundreds of lines of code in JavaScript.

```python
# Create a chart of NDVI over time
geemap.chart_image_collection_by_day(
    processed_collection, 
    'NDVI', 
    region=roi, 
    scale=30
)
```

---

## 🖼️ Section 4: Exporting & Publishing

### 4.1 Exporting to Local Drive

Standard GEE exports to Google Drive. `geemap` allows for direct downloading of small-to-medium datasets.

```python
# Download as GeoTIFF directly to your Downloads folder
geemap.ee_export_image(
    median_image.select(['B4', 'B3', 'B2']), 
    filename="my_satellite_export.tif", 
    scale=30, 
    region=roi
)
```

### 4.2 Building Interactive Web Apps

You can convert a Jupyter notebook into a full web application using **Streamlit** + **geemap**.

```python
import streamlit as st
import geemap.foliumap as geemap

st.title("Sentinel-2 Explorer")
m = geemap.Map()
m.add_basemap("HYBRID")
m.to_streamlit()
```

---

## 🛠️ Advanced geemap Utilities

| Utility | Description | Code Example |
| :--- | :--- | :--- |
| **JS to Python** | Converts official GEE docs (JS) to Python | `geemap.js_to_python(in_file, out_file)` |
| **Zonal Stats** | Calculate stats for thousands of polygons | `geemap.zonal_stats(roi, image, stats_type='MEAN')` |
| **Time-Lapse** | Create animated GIFs of urban expansion | `geemap.sentinel2_timelapse(roi, ...)` |
| **Split Map** | Compare two images side-by-side | `Map.split_map(left_layer, right_layer)` |

---

## ❓ Deep Dive: Troubleshooting the Cloud

**Q: Why do I get "Computed value is too large" error?**
A: This happens when you try to use `.getInfo()` on a large object (like a global image). GEE prevents these large data transfers. Instead, **Export** the result to Google Drive.

**Q: My `Map.addLayer` is taking forever!**
A: Check if you have complex geometry (thousands of vertices). Try simplifying your geometry using `.simplify(10)` or reduce the number of images being processed simultaneously.

**Q: Can I use PyTorch models in GEE?**
A: Yes! This is an advanced workflow using **Google AI Platform**. You train the model locally, host it in the cloud, and GEE sends data to the model for real-time inference.

---

!!! warning "Quota Limits"
    GEE is free for research but has strictly enforced rate limits. Avoid putting `.getInfo()` inside a local Python `for` loop, as it will swamp the API and get you temporarily blocked.

[Next: Machine Learning & Geospatial Intelligence &raquo;](ml_geospatial.md){ .md-button .md-button--primary }
