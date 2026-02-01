# 🗺️ Geospatial Coding Hub

A comprehensive, professional-grade guide and curriculum for mastering geospatial data science, from local Python processing to planetary-scale cloud computing.

[![Publish to GitHub Pages](https://github.com/pulakeshpradhan/geospatial/actions/workflows/publish.yml/badge.svg)](https://github.com/pulakeshpradhan/geospatial/actions/workflows/publish.yml)
[![Live Site](https://img.shields.io/badge/Live-Site-blue?style=for-the-badge&logo=github)](https://pulakeshpradhan.github.io/geospatial/)

---

## 🚀 Overview

This repository houses the source code and documentation for the **Geospatial Coding Hub**. It is designed to bridge the gap between GIS technicians and Geospatial Data Scientists by providing rigorous, code-first tutorials.

### ✨ Key Features

- **Python for GIS**: Mastery of `GeoPandas`, `Rasterio`, `Shapely`, and `PySAL`.
- **Modern Data Sources**: Deep dive into **STAC**, **COG**, **GeoParquet**, and **Zarr**.
- **Cloud Computing**: Leveraging **Google Earth Engine (GEE)** and `geemap` for big data analysis.
- **GeoAI**: Machine Learning (Random Forest) and Deep Learning (U-Net) workflows for satellite imagery.
- **Automated Deployment**: Built with MkDocs Material and deployed via GitHub Actions.

---

## 🛠️ Getting Started

### 1. Fast Setup with Conda/Mamba

We recommend using the provided `environment.yml` to ensure all complex geospatial dependencies (GDAL, GEOS, PROJ) are installed correctly.

```bash
# Clone the repository
git clone https://github.com/pulakeshpradhan/geospatial.git
cd geospatial

# Create the environment
conda env create -f environment.yml

# Activate it
conda activate geo_master
```

### 2. Local Documentation Preview

To view the site locally before pushing:

```bash
pip install -r requirements.txt
mkdocs serve
```

The site will be available at `http://127.0.0.1:8000`.

---

## 📂 Repository Structure

- `docs/`: Markdown source files for the documentation.
- `.github/workflows/`: CI/CD pipeline for automatic deployment.
- `site/`: (Ignored) Built static site content.
- `mkdocs.yml`: Configuration for the MkDocs site layout and theme.

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## ⚖️ License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 👤 Author

**Pulakesh Pradhan**  

- GitHub: [@pulakeshpradhan](https://github.com/pulakeshpradhan)
- Website: [pulakeshpradhan.github.io/geospatial/](https://pulakeshpradhan.github.io/geospatial/)

---

*“The use of GIS is limited only by the imagination of those who use it.” — Jack Dangermond*
