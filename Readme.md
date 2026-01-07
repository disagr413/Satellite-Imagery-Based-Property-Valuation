**Satellite Imagery–Based Property Valuation**

**1. Project Title**

Multimodal Property Price Prediction Using Tabular Data and Satellite Imagery

**2. Project Overview**

This project develops a regression framework to predict residential property prices by combining structured housing attributes with satellite imagery–derived features. The approach integrates domain-driven tabular features and visual neighbourhood context to capture both property-level and location-level drivers of value.

**3. Problem Statement**

Traditional property valuation models rely heavily on structured attributes such as size, quality, and location. However, these features fail to fully capture neighbourhood characteristics like urban density, open space, and surrounding infrastructure.
This project investigates whether incorporating satellite imagery can incrementally improve price prediction beyond tabular data alone.

**4. Dataset Description**

The dataset consists of residential properties with:

**Tabular data:**
- Structural attributes (bedrooms, bathrooms, floors, grade, condition)
- Size-related features (living area, lot size, basement area)
- Temporal features (house age)
- Geographic coordinates (latitude, longitude, zipcode)

**Satellite imagery:**
- High-resolution NAIP images centered around each property location.
- Images aligned using property latitude–longitude coordinates.

The dataset is split into training, validation datasets.

**5. Methodology / Model Pipeline**

The modelling pipeline follows these stages:
- Data cleaning and preprocessing
- Exploratory data analysis (EDA)
- Tabular feature engineering
- Satellite image feature extraction
- Dimensionality reduction of image embeddings
- Gradient boosting regression (XGBoost)
- Epoch-wise validation and model comparison

**6. Feature Engineering**

**Tabular Feature Engineering:**
- Log transformation applied to highly skewed size-related features to stabilize variance.
- Standardization applied to numerical inputs for scale consistency.
- Domain-driven features such as:
         - house_age_before_sale
         - total_sqft
         - zipcode_offset (zipcode – 98000) as a compact spatial proxy
- Categorical features retained in raw form for tree-based learning.

**Image-Based Feature Engineering:**
- CNN embeddings extracted using a pretrained ResNet18.
- Structural density features engineered from satellite images:
         - Building density q75
         - Road density q75
         - Open space variance
         - Density balance
         - Water presence flag
- PCA reduction applied to compress 512-dimensional CNN embeddings into 16 components, reducing noise and multicollinearity.

**7.Model Details**

**Model:** XGBoost Regressor

**Target:** price_log (log-transformed property price)

**Training strategy:**
- Epoch-wise validation using iteration_range
- Early performance selection based on validation RMSE

**Setup:**
Baseline: tabular features only
Multimodal model: tabular + image-derived features

**8.Evaluation Metrics and Results**

Models are evaluated on the validation set using:

**RMSE (log scale)**:   

         -**Tabular_only(Baseline)**: 0.1637 (Cross Validation)
         -**Multimodal**: 0.1626

**R² (log scale)**:    

         -**Tabular_only(Baseline)**: 0.9022 (Cross Validation)    
         -**Multimodal**: 0.9042

Log transformation of the target is used to:
- Reduce right-skewness
- Minimize outlier leverage
- Stabilize model learning

**RMSE (Real)**:

         -**Tabular_only(Baseline)**: 117830.3588 (Cross Validation)
         -**Multimodal**: 115847

**R² (Real)**: 

         -**Tabular_only(Baseline)**: 0.8928   (Cross Validation)    
         -**Multimodal**: 0.8931

**Insights**: 
- Tabular features explain the majority of price variance.
- Satellite imagery provides incremental but consistent improvement.
- Image-derived features act as refinement signals, explaining residual variation among structurally similar properties.

**9.Project Structure**

├── preprocessing.ipynb
├── model_training.ipynb
├── data_fetcher.py
├── 24113039_report.pdf
├── 24113039_final.csv
├── README.md

**10. How to Run the Project**

This project follows a **multi-stage workflow** executed across **Colab environments and Kaggle**, and is **not** a single end-to-end pipeline.  
The steps must be run **sequentially** as outlined below.

**Step 1: Data Cleaning, EDA and Feature Engineering (Colab)**
- Run the **preprocessing.ipynb notebook** to:
  - Perform basic cleaning and sanity checks.
  - Engineer tabular features (log transforms, scaling, spatial features).
- Export the fully processed dataset as CSV files.
- Output of this step:
  - Cleaned raw tabular data.
  - Final processed train datasets.

**Step 2: Raw Data Fetching (Colab)**
- Run the **data_fetcher.py notebook** to:
  - Load the raw tabular housing dataset.
  - Fetch satellite imagery (GeoTIFFs) using geographic coordinates.
  - Alignment of GeoTIFF images in the excel sheet, with respect to 'id'.
- Output of this step:
  - Downloaded raw satellite images.
  - Final processed test datasets.
- Final modelling ready files name is traindata_process and testdata_process.

**Step 3: Upload Processed Data to Kaggle**
- Upload the exported processed datasets to a **Kaggle Dataset**.
- Ensure the dataset is accessible to Kaggle notebooks for training.

**Step 4: Model Training & Evaluation (Kaggle)**
- Open the **model_training.ipynb notebook** on Kaggle.
- Convert satellite images to model-ready formats.
- Train:
  - Tabular-only XGBoost model
  - Multimodal model (tabular + satellite image features)
- Perform epoch-wise validation and metric comparison
- Generate final evaluation plots and results

⚠️ **Important:**  
Each step depends on artifacts generated in the previous step.  
Skipping or reordering steps will break the workflow.

**11. Limitations**

- Satellite image features provide modest gains due to dominance of strong tabular predictors.
- CNN is used as a fixed feature extractor (no end-to-end training).
- Image resolution and temporal alignment may limit fine-grained neighbourhood capture.

**12. Future Work**

- End-to-end CNN training directly on price supervision.
- Higher-resolution or multi-temporal imagery.
- Advanced spatial encodings (graph-based or geospatial kernels).
- SHAP-based multimodal explainability



**Author:** Disha Agrawal 

**Enrollment No. :** 24113039


**Institution:** Indian Institute of Technology, Roorkee
