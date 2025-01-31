
# GEELULC: Land Use Land Cover Classification Using Google Earth Engine

This repository contains code for performing Land Use Land Cover (LULC) classification using various machine learning algorithms in Google Earth Engine (GEE). The classification is based on Sentinel-2 Surface Reflectance data. The repository includes scripts for multiple classification models, such as Support Vector Machine (SVM), Minimum Distance (MD), Classification and Regression Trees (CART), Gradient Tree Boosting (GBM), K-Nearest Neighbors (KNN), Naïve Bayes (NB), and Random Forest (RF), with various configurations for performance evaluation.

## Repository Structure
```
GEELULC/
│
├── assets/                        # Contains shapefiles and other required assets
│   ├── AOI.shp                    # Area of Interest shapefile
│   ├── AOI.dbf                    # Associated DBF file for AOI
│   ├── AOI.prj                    # Projection file for AOI
│   └── tpoints.shp                # Training points shapefile
│
├── classifiers/                   # Folder containing classifier scripts
│   ├── SVM_Linear_Classifier.js   # SVM Linear Kernel (Case 1)
│   ├── SVM_Poly_Classifier.js     # SVM Polynomial Kernel (Case 2)
│   ├── SVM_RBF_Classifier.js      # SVM RBF Kernel (Case 3)
│   └── other_classifiers.js       # Additional classifier scripts for different models
│
└── README.md                      # This user manual
```

## Prerequisites

- **Google Earth Engine (GEE) Account**: You need an active GEE account to run the scripts. To get access, sign up [here](https://signup.earthengine.google.com/).
- **Shapefiles**: Upload the shapefiles to your GEE assets (e.g., Area of Interest and Training Points).

## Uploading Assets to GEE

Before running the scripts, you need to upload the following assets to GEE:

1. **Area of Interest (AOI)**: Upload the shapefile (`AOI.shp`, `AOI.dbf`, `AOI.prj`) as a `FeatureCollection` named `table`.
   
   **Steps**:
   - Go to your GEE Code Editor [here](https://code.earthengine.google.com/).
   - In the "Assets" tab, click on "New" > "Shapefile" and upload the files.
   - After uploading, the asset will be available in the GEE Code Editor as a `FeatureCollection`.

2. **Training Points**: Upload the training points shapefile (`tpoints.shp`, `tpoints.dbf`, `tpoints.prj`) as a `FeatureCollection` named `tpoints`.

   **Steps**:
   - Follow the same steps as for the AOI shapefile, ensuring it is uploaded with the correct name `tpoints`.

Once these assets are uploaded, ensure that they are correctly referenced in your script (see below).

## Running the Classification

The classification scripts are contained in the `classifiers/` folder. You can run these scripts directly in Google Earth Engine using the GEE Code Editor. Each script contains code for training and applying a specific machine learning algorithm on the Sentinel-2 Surface Reflectance data.

### Example: Running the SVM Classifier (Case 8)

1. **Open the script**: Open `SVM_Poly_Classifier.js` (or any other classifier script) in the GEE Code Editor.
   
2. **Set the AOI and training points**:
   - Make sure the AOI (`table`) and training points (`tpoints`) are uploaded to GEE as assets.
   - If you uploaded them under different names, change the asset references in the code accordingly:
     ```javascript
     var table = ee.FeatureCollection('projects/your_project/assets/AOI');
     var trainingPoints = ee.FeatureCollection('projects/your_project/assets/tpoints');
     ```

3. **Run the script**: Click the "Run" button in the GEE Code Editor. The script will:
   - Load Sentinel-2 Surface Reflectance data for the specified date range (e.g., May 2024).
   - Apply a machine learning classifier (e.g., SVM) to classify the land cover based on the training points.
   - Display the classified result on the map.
   - Optionally, export the classified image to Google Drive.

### Exporting the Results

The classified land cover image is exported to Google Drive as a GeoTIFF file. Ensure that you have sufficient space in your Google Drive to store the output.

To export the classification, the code uses the following command:
```javascript
Export.image.toDrive({
  image: classified,
  description: 'LULC_Output',
  folder: 'GEE_Files',
  fileNamePrefix: 'LULC_Classification',
  scale: 30,
  region: table.geometry(),
  maxPixels: 1e13
});
```

The result will be available in the folder `GEE_Files` on your Google Drive.

## Accuracy Assessment

The script also computes the accuracy of the classification using a confusion matrix. The confusion matrix includes metrics such as:
- **Overall Accuracy**
- **Kappa Coefficient**
- **Producer’s Accuracy**
- **Consumer’s Accuracy**

You can print these metrics by running the script, and they will be displayed in the GEE console.

```javascript
print('Confusion matrix:', confusionMatrix);
print('Overall Accuracy:', confusionMatrix.accuracy());
print('Kappa Coefficient:', confusionMatrix.kappa());
print('Producers Accuracy:', confusionMatrix.producersAccuracy());
print('Consumers Accuracy:', confusionMatrix.consumersAccuracy());
```

## Available Classifiers and Configurations

The repository includes the following classifiers with different configurations:

1. **Support Vector Machine (SVM)**:
   - Linear Kernel (Cost = 1)
   - Polynomial Kernel (Gamma = 0.5, Cost = 1, Degree = 3)
   - RBF Kernel (Gamma = 0.1, Cost = 10)
   - Sigmoid Kernel (Gamma = 0.1, Cost = 5)

2. **Other Classifiers**:
   - Minimum Distance (MD)
   - Classification and Regression Trees (CART)
   - Gradient Tree Boosting (GBM)
   - K-Nearest Neighbors (KNN)
   - Naïve Bayes (NB)
   - Random Forest (RF)

You can switch between different classifiers by changing the script accordingly.

## Citation

If you use this repository in your research, please cite it as:

```
Chaturvedi, S. (2025). GEELULC: Land Use Land Cover Classification Using Google Earth Engine. Retrieved from [https://github.com/Shobhite73/GEELULC/tree/main]
```

