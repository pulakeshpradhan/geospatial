# Python for GIS

Python is the backbone of modern geospatial analysis. Here are the core libraries you should know.

## 📦 Essential Libraries

### 1. GeoPandas

GeoPandas makes working with geospatial data in python easier by extending datatypes used by `pandas`.

```python
import geopandas as gpd

# Load a shapefile
df = gpd.read_file('data.shp')
print(df.head())
```

### 2. Rasterio

For reading and writing geospatial raster data.

```python
import rasterio
with rasterio.open('image.tif') as dataset:
    print(dataset.width, dataset.height)
```

### 3. PyProj

For cartographic projections and coordinate transformations.
