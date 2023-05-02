# AMP--Parkinson-s_Disease-Progression_Prediction
Data Source: https://www.kaggle.com/competitions/amp-parkinsons-disease-progression-prediction/overview
Problem Statement: 
Parkinson’s disease (PD) is a disabling brain disorder that affects movements, cognition, sleep, and other normal functions
Research indicates that protein or peptide abnormalities play a key role in the onset and worsening of this disease
Goal:  predict MDS-UPDR scores (from updrs 1 to 4), which measure progression in patients with Parkinson's disease
Data Baground:
Available data:
Training data:
train_peptides.csv Mass spectrometry data at the peptide level.
train_proteins.csv Protein expression frequencies aggregated from the peptide level data.
train_clinical_data.csv (include patient’s score per PD rating scale)
Testing data:
test_peptides.csv
test_proteins.csv
test.csv
Data overview:
Input features: visit_month, NPX, PeptideAbundance
Potential target variable(s): updrs_1, updrs_2, updrs_3, updrs_4

DATA PREPROCESSING:
Missing values: 
For numerical data: replacing missing values with their corresponding column arithmetic average
For non-numerical data: replacing missing values with the column mode
Outliers removal: defined first and third quantiles, and removed any data points that are outside of the upper and lower bounds of the threshold 
Feature Selection: dropping columns like “visit_id”, “upd23b_clinical_state_on_medication”, “UniProt”, and “Peptide”
Merging:
Inner join between “train_peptides” and “train_proteins” dataset on “visit_month”, “UniProt” and “patient_id” columns as “train”
Inner join between “train” and “train_clincal_data” dataset on “visit_id” and “patient_id” columns
All UPDRS scores are left skewed with outliers mostly centered on the right
This may show that the most of the patients are not in the severe impairment level
Highest correlation score is between UPDRS_2 and UPDRS_3
Lowest correlation score is between UPDRS_4 and UPDRS_3 
UPDRS scores with medication status has shown that the ones without missing record are similarly distributed
UPDRS scores with medication status has shown that the ones with missing record (or Null values) are centered to the far left. 

Methodologys
LSTM Model:
We used an LSTM model to predict MDS-UPDR scores based on the preprocessed data. The LSTM model was trained on sequential data, where each patient's previous MDS-UPDR scores were used to predict the next score. We used a sliding window approach to create sequences of fixed length from the data.
LightGBM Model:
We then used a LightGBM model to further refine the LSTM predictions. The LightGBM model was trained on the same data used for the LSTM model, along with other relevant patient features. This allowed us to capture additional patterns and relationships in the data that the LSTM model may have missed.
M5 Prime:
Regression tree built on M5 Algorithm developed by Quinlan (1992). Each leaf node is used to generated a linear regression model. Steps include: Splitting criterion, pruning, smoothing. It has been known to perform well in predicting nonlinear and noisy datasets.
Results:
We chose UPDRS3 as the target variable due to its highest number of unique values (58) among the UPDRS scores.
We trained the data using LSTM, LightGBM, and M5 Prime models.
LSTM:
Train_time = approximately 8 hours
RMSE = 14.75
MAE = 12.11
r2_score = 0.0289
LightGB:
Train_time = 2mins
RMSE = 13.4
MAE = 11.29
r2_score = 0.0719
m5 Prime:
Train_time = 48mins
MSE = 15.18
r2_score = 0.92
Conclusion:
Based on the given performance metrics, it appears that the M5 Prime model has the best overall performance, with the highest r2_score of 0.92, indicating that it explains 92% of the variance in the data. The LightGB model has the next best performance with an r2_score of 0.0719, while the LSTM model performs the worst with an r2_score of 0.0289.
It is also worth noting that the LightGB model has a significantly faster training time (2 minutes) compared to the LSTM model (approximately 8 hours), which may be an important consideration depending on the specific use case and available computational resources.
In conclusion, based on the provided metrics, the M5 Prime model is the best-performing model, followed by the LightGB model, and the LSTM model performs the worst.
