# Python for Geospatial Analysis

Python is the most popular language for professional GIS. This guide covers the essential stack for vector and raster data manipulation.

## 🏗️ The Vector Stack (GeoPandas & Friends)

Vector data (Shapefiles, GeoJSON) is best handled using **GeoPandas**, which extends the popular `pandas` library.

=== "Data Loading"

    ```python
    import geopandas as gpd

    # Read from file
    gdf = gpd.read_file("counties.shp")

    # Inspect the spatial data
    print(gdf.head())
    print(gdf.crs) # Check Coordinate Reference System
    ```

=== "Spatial Analysis"

    ```python
    # Buffer analysis (create 500m zone)
    gdf['buffer'] = gdf.geometry.buffer(500)

    # Spatial Join: Join points to polygons
    hospitals_in_counties = gpd.sjoin(hospitals, gdf, how="inner", predicate='within')
    ```

=== "Visualization"

    ```python
    # Simple thematic map
    gdf.plot(column='population', cmap='OrRd', legend=True)
    ```

---

## 🖼️ The Raster Stack (Rasterio & Xarray)

Raster data (GeoTIFFs, Satellite imagery) is handled using **Rasterio** and **Xarray** for multi-dimensional arrays.

=== "Metadata"

    ```python
    import rasterio

    with rasterio.open('landsat_b4.tif') as src:
        print(src.width, src.height)
        print(src.bounds)
        print(src.transform) # Geotransform matrix
    ```

=== "Band Calculation (NDVI)"

    ```python
    import numpy as np

    with rasterio.open('image.tif') as src:
        red = src.read(3) # Band 3
        nir = src.read(4) # Band 4
        
        # Calculate NDVI
        ndvi = (nir.astype(float) - red) / (nir + red)
    ```

---

## 🛠️ Environment Setup

We recommend using **Conda** or **Mamba** to manage geospatial dependencies, as they can be tricky to install via pip.

```bash
# Recommended environment setup
conda create -n geospatial -c conda-forge geopandas rasterio geemap leafmap
conda activate geospatial
```

!!! info "Pro Tip"
    Always use `conda-forge` for geospatial libraries to avoid library version conflicts (DLL hell).
