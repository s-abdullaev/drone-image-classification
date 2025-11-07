# RF Image Classification for UAV Orthomosaics

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/s-abdullaev/drone-image-classification/blob/main/rf_classification.ipynb)

## Overview

This notebook implements a complete Random Forest (RF) classification pipeline for UAV (drone) orthomosaic imagery. The workflow guides you through supervised classification of RGB imagery, from loading data to producing a classified land cover map.

## Features

- RGB orthomosaic image processing
- Calculation of RGB-based vegetation indices (VARI, ExG, NDI, GRVI)
- Training data extraction from QGIS geopackages
- Random Forest model training with 301 trees
- Comprehensive accuracy assessment with confusion matrices
- Full image classification and visualization
- Model persistence (save/load trained models)

## Requirements

The notebook uses the following Python libraries:

```
rasterio
rioxarray
xarray
geopandas
pandas
numpy
matplotlib
seaborn
scikit-learn
plotly
pickle
```

## Dataset

The notebook expects the following files in the same directory:

1. **Orthomosaic Image**: A georeferenced RGB orthomosaic in GeoTIFF format (3-band, projected CRS)
   - Example: `Ortho3GCPsEPSG6342.tif`

2. **Training Data**: A geopackage containing training polygons with a 'class' attribute
   - Example: `classes.gpkg`

## Usage

1. Place your orthomosaic `.tif` file in the notebook directory
2. Place your training data `.gpkg` file in the notebook directory
3. Update the file names in the notebook:
   - Cell 3: Change `img_path = "Ortho3GCPsEPSG6342.tif"` to your orthomosaic filename
   - Cell 5: Change `gpd.read_file("classes.gpkg")` to your training data filename
4. Run all cells sequentially

## Workflow

### 1. Data Loading
- Loads the UAV orthomosaic using rioxarray
- Validates that the image has only RGB bands (removes alpha channel if present)
- Loads training polygons from QGIS geopackage

### 2. Index Calculation
Creates RGB-based vegetation indices:
- **VARI** (Visible Atmospherically Resistant Index)
- **ExG** (Excess Green)
- **NDI** (Normalized Difference Index, Blue-Red)
- **GRVI** (Green-Red Vegetation Index)

### 3. Training Data Extraction
- Extracts pixel values from training polygons
- Provides sample size statistics per class

### 4. Data Visualization
- Histograms showing spectral distributions by class
- Density plots overlaying all classes
- Boxplots for each band/index by class

### 5. Model Training
- Splits data into 80% training / 20% testing
- Trains Random Forest classifier with 301 trees
- Displays feature importance rankings

### 6. Accuracy Assessment
- Confusion matrix (normalized by row)
- Overall accuracy score
- Per-class precision, recall, and F1-scores
- Detailed classification report

### 7. Full Image Classification
- Applies trained model to entire orthomosaic
- Saves classified map as `classified_map_python.tif`
- Visualizes final classification results

**Note:** Full image classification can take 20-30 minutes depending on image size and computer specs.

## Output Files

- `my_trained_rf_model.pkl` - Trained Random Forest model
- `classified_map_python.tif` - Final classified raster map

## Tips for Better Results

1. **Training Data Quality**:
   - Use 4-5 distinct classes minimum
   - Create many small polygons rather than few large ones
   - Ensure each polygon is homogeneous (pure class)

2. **Class Balance**:
   - Check sample sizes in the confusion matrix
   - Add more training polygons for underrepresented classes

3. **Iterative Refinement**:
   - Review confusion matrix for misclassifications
   - Return to QGIS to improve training polygons for poorly predicted classes
   - Consider merging classes that are too similar

## Questions to Answer

After running the classification, answer the following questions:

1. **Q1 (5 pts):** Describe the process for supervised classification. Within this, briefly explain the random forest approach.

2. **Q2 (5 pts):** Discuss how well your classes are defined (what classes did you use and why?). Use the spectral plots for this answer.

3. **Q3 (5 pts):** Connected to Q2, how well does the model perform? Assess it using the tables and graphs that you generated (Variable Importance and Confusion Matrix).

4. **Q4 (4 pts):** Discuss your classification results in general outside of the training data evaluation. What were issues/difficulties during the classification process? How could you improve the classification?

5. **Q5 (5 pts):** In addition to answering the questions, include the resulting notebook file. That should ideally have all the plots and figures.

## Troubleshooting

- **Long file paths**: Python may have issues with deeply nested folders. Consider moving the project to Desktop or a shorter path.
- **Memory issues**: If classification crashes, the image may be too large for available RAM.
- **Classification takes too long**: Consider processing a subset of the image first, or run on a more powerful machine.

## License

This notebook was developed for educational purposes as part of the UWyo Drone-based Remote Sensing course.

## Contact

For questions or issues, please contact your course instructor.
