# Professional Learning Path

Use these structured modules to master geospatial coding. Each level builds upon the previous one.

<div class="tutorial-grid">

<div class="tutorial-card capsule" markdown>
### 🟢 Level 1: Foundations
Master the basics of data structures and environment setup.
*   [Setting up Conda/Mamba](python_gis.md)
*   [Introduction to GeoPandas](python_gis.md)
*   [Basic Vector Operations](python_gis.md)
</div>

<div class="tutorial-card capsule" markdown>
### 🟡 Level 2: Cloud Computing
Transition your local workflows to the cloud for planetary scale.
*   [Introduction to GEE](gee_geemap.md)
*   [Sentinel-2 Data Exploration](gee_geemap.md)
*   [Time-series Analysis](gee_geemap.md)
</div>

<div class="tutorial-card capsule" markdown>
### 🔴 Level 3: Advanced Intelligence
Integrate your spatial skills with predictive modeling.
*   [Random Forest for LULC](ml_geospatial.md)
*   [Unsupervised Clustering](ml_geospatial.md)
*   [Statistical Modeling (PR/Logit)](ml_geospatial.md)
</div>

</div>

---

## 📈 Completion Guide

Check off your achievements as you progress through the hub:

- [ ] Install local geospatial environment
- [ ] Create your first Buffer Map in Python
- [ ] Authenticate GEE on your machine
- [ ] Run a classification model on GEE

!!! quote "Geospatial Wisdom"
    "The use of GIS is limited only by the imagination of those who use it." — *Jack Dangermond*

<style>
.tutorial-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 1.5rem;
    margin: 2rem 0;
}
.tutorial-card {
    padding: 1.5rem;
    border: 1px solid #e2e8f0;
    border-radius: 12px;
    background: #f8fafc;
    transition: all 0.3s ease;
}
.tutorial-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
    border-color: #3b82f6;
}
.tutorial-card h3 {
    margin-top: 0 !important;
}
</style>
