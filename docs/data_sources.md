# Modern Geospatial Data Sources & Formats

The landscape of geospatial data has shifted from local "file-based" workflows to "cloud-native" architectures. Instead of downloading gigabytes of zip files, we now query metadata and stream only the pixels or features we need.

---

## 🛰️ 1. STAC (SpatioTemporal Asset Catalog)

**STAC** is a specification that provides a common language to describe geospatial information. It makes it easier to index and discover data from multiple providers (NASA, ESA, Planet, etc.).

### Why use STAC?

- **Searchability:** Find data by time, location, and property (e.g., "Cloud cover < 10%").
- **Asset Links:** Direct URLs to files (COGs, metadata, thumbnails).
- **Interoperability:** The same code works for Sentinel-2, Landsat, and commercial data.

### 🐍 Python Example: Searching for Data

Using `pystac-client` to find Sentinel-2 images over a specific area.

```python
from pystac_client import Client

# Microsoft Planetary Computer STAC API
catalog = Client.open("https://planetarycomputer.microsoft.com/api/stac/v1")

# Define our area of interest (Bhubaneswar, India)
area_of_interest = {
    "type": "Point",
    "coordinates": [85.8245, 20.2961]
}

# Search for Sentinel-2 data in 2023 with low cloud cover
search = catalog.search(
    collections=["sentinel-2-l2a"],
    intersects=area_of_interest,
    datetime="2023-01-01/2023-12-31",
    query={"eo:cloud_cover": {"lt": 5}}
)

items = search.item_collection()
print(f"Found {len(items)} items matching your criteria.")

# Access the first item's assets
for asset_key, asset in items[0].assets.items():
    print(f"- {asset_key}: {asset.title}")
```

---

## 🖼️ 2. Cloud Optimized GeoTIFF (COG)

A **COG** is a regular GeoTIFF file but with a specific internal organization that allows for "HTTP Range Requests." This means you can read just a small portion of a huge image without downloading the whole thing.

### Key Features

- **Internal Tiling:** Data is stored in small chunks.
- **Overviews (Pyramids):** Pre-rendered low-resolution versions for fast viewing.
- **Streaming:** Works perfectly with `rasterio` and `stackstac`.

### 🐍 Python Example: Streaming a COG

```python
import stackstac
import pystac_client

# Get a STAC item (from the previous example)
item = items[0]

# Load only the Red and NIR bands into an Xarray DataArray
# stackstac handles the "lazy loading" from the cloud
ds = stackstac.stack(item, assets=["red", "nir"])

print(ds)
# The data isn't downloaded yet! It's just metadata until you call .compute()
```

---

## 🧬 3. Modern Vector Formats: GeoParquet & FlatGeobuf

Traditional Shapefiles are outdated (limitations on file size, column names, and multi-file structure). Modern alternatives are designed for the cloud.

| Format | Best For | Why? |
| :--- | :--- | :--- |
| **GeoParquet** | Big Data / Analytics | Columnar storage, high compression, works with Spark/DuckDB. |
| **FlatGeobuf** | Web Mapping / Streaming | Fast spatial indexing, can be read incrementally over HTTP. |
| **GeoJSON** | Small Data / Web | Easy to read, but slow for large datasets. |

### 🐍 Python Example: GeoParquet

```python
import geopandas as gpd

# Reading a GeoParquet file from a URL
url = "https://example.com/data.parquet"
gdf = gpd.read_parquet(url)

# Writing to GeoParquet (requires pyarrow)
gdf.to_parquet("my_data.parquet", compression='snappy')
```

---

## 🧊 4. Multidimensional Data: Zarr

For climate data or time-series (like 50 years of daily rainfall), **Zarr** is the cloud-native alternative to NetCDF/HDF5.

- **Chunked:** Allows parallel reads across many threads.
- **Scalable:** Stores data in chunks as separate files or objects in S3/Azure Blob.
- **Integration:** Native support in `Xarray`.

### 🐍 Python Example: Zarr

```python
import xarray as xr

# Open a Zarr dataset from Google Cloud Storage
ds = xr.open_zarr("gs://cmip6/CMIP6/CMIP/AS-RCEC/TaiESM1/historical/r1i1p1f1/Amon/tas/gn/v20200225/")
print(ds)
```

---

## 📚 Summary Table: What to use?

| Use Case | Recommended Format | Tool to Use |
| :--- | :--- | :--- |
| **Finding Imagery** | STAC | `pystac-client` |
| **Satellite Pixels** | COG | `stackstac`, `rasterio` |
| **Big Vector Data** | GeoParquet | `geopandas`, `dask-geopandas` |
| **Weather/Climate** | Zarr | `xarray`, `zarr` |
| **Web App Streaming** | MVT (Vector Tiles) | `tippecanoe`, `maplibre` |

---

!!! tip "Workflow Tip"
    Always try to use **STAC** as your entry point for data. It abstracts away the complexity of where files are stored and how they are named.

[Next: Cloud Computing with GEE & Geemap &raquo;](gee_geemap.md){ .md-button .md-button--primary }
