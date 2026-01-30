# Machine Learning for Geospatial Data

Machine learning (ML) provides powerful techniques for analyzing spatial patterns, classifying land cover, and predicting environmental changes.

## 🌲 Random Forest Classification

Random Forest is one of the most popular algorithms for Land Use/Land Cover (LULC) classification due to its robustness and ability to handle high-dimensional satellite data.

=== "Python"

    ```python
    from sklearn.ensemble import RandomForestClassifier
    import pandas as pd

    # Load training data (features + labels)
    data = pd.read_csv("training_data.csv")
    X = data[['band1', 'band2', 'band3', 'band4']] # Spectral Bands
    y = data['land_cover_class']                  # Class Labels

    # Initialize and Train
    rf = RandomForestClassifier(n_estimators=100, random_state=42)
    rf.fit(X, y)

    # Predict on new data
    predictions = rf.predict(new_spectral_data)
    ```

=== "R"

    ```r
    library(randomForest)

    # Load training data
    data <- read.csv("training_data.csv")
    
    # Train Random Forest Model
    # Formula: labels ~ features
    rf_model <- randomForest(as.factor(land_cover_class) ~ band1 + band2 + band3 + band4, 
                             data = data, 
                             ntree = 100)

    # Predict on new data
    predictions <- predict(rf_model, new_spectral_data)
    ```

---

## 🛰️ K-Means Clustering (Unsupervised)

Unsupervised learning is useful for exploratory data analysis and identifying natural groupings in satellite imagery without training labels.

=== "Python"

    ```python
    from sklearn.cluster import KMeans
    import numpy as np

    # Assume 'image_pixels' is a flattened array of band values
    kmeans = KMeans(n_clusters=5, random_state=0)
    clusters = kmeans.fit_predict(image_pixels)

    # Reshape back to image dimensions
    clustered_map = clusters.reshape(height, width)
    ```

=== "R"

    ```r
    # Load spectral data
    spectral_matrix <- as.matrix(spectral_data)

    # Run K-Means Clustering
    km_result <- kmeans(spectral_matrix, centers = 5, nstart = 25)

    # Get cluster assignments
    clusters <- km_result$cluster
    ```

---

## 📈 Logistic Regression for Susceptibility

Often used in geospatial science for predicting binary outcomes like landslide susceptibility or deforestation probability.

=== "Python"

    ```python
    from sklearn.linear_model import LogisticRegression

    # Features: Slope, Aspect, Rainfall, Land Cover
    X = data[['slope', 'aspect', 'rainfall', 'lulc']]
    y = data['occurrence'] # 0 or 1

    model = LogisticRegression()
    model.fit(X, y)

    # Probability of occurrence
    prob = model.predict_proba(X)
    ```

=== "R"

    ```r
    # Train Logistic Regression Model (GLM)
    logit_model <- glm(occurrence ~ slope + aspect + rainfall + lulc, 
                       data = data, 
                       family = "binomial")

    # Summary of coefficients
    summary(logit_model)

    # Predict probabilities
    probabilities <- predict(logit_model, type = "response")
    ```

!!! info "Choosing the Language"
    While **Python** is the industry standard for deep learning (PyTorch/TensorFlow) and large-scale cloud processing (GEE), **R** remains exceptionally strong for statistical spatial analysis and high-quality cartographic plotting (`ggplot2`, `sf`).
