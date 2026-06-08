
Micro Injection Moulding Analysis
This repository contains a Jupyter notebook for analyzing microscopic measurement data, focusing on predicting micron values based on various experimental parameters and signal characteristics.

Table of Contents
Introduction
Environment Setup
Data Processing
1kHz Frequency Data
10kHz Frequency Data
Exploratory Data Analysis (EDA)
Peak Detection and Data Segmentation
Feature Engineering and Aggregation
Model Training and Evaluation
Conclusion
Usage
1. Introduction
This notebook demonstrates a comprehensive workflow for analyzing microscopic measurement data. The primary goal is to process raw signal data from different temperatures and frequencies, extract relevant features, and build machine learning models to predict micron values. The analysis includes data loading, preprocessing, feature engineering, exploratory data analysis, and advanced model training with uncertainty quantification.

2. Environment Setup
This section covers the installation of necessary Python libraries and initial setup steps required to run the notebook. Key libraries include pandas, numpy, matplotlib, seaborn, openpyxl, scipy, scikit-learn, tinygp, and scikit-optimize.

3. Data Processing
The raw data is provided in .zip archives, categorized by temperature (50°C, 65°C, 80°C) and frequency (1kHz, 10kHz). The processing steps involve unzipping these archives, loading individual CSV files into pandas DataFrames, cleaning and renaming columns, adding relevant metadata (Temperature, Frequency, Time), and concatenating them into comprehensive DataFrames.

1kHz Frequency Data
Data for the 1kHz frequency is processed separately, including renaming files for sequential order and merging with corresponding microscopic measurement Excel data (microscope_measurements_excel_1kHz.xlsx).

10kHz Frequency Data
Similar to the 1kHz data, 10kHz frequency data undergoes unzipping, loading, cleaning, and merging with its respective microscopic measurement Excel data (microscope_measurements_excel.xlsx). Additional steps involve dropping irrelevant columns (Cavity_Peak, Cavity_P).

4. Exploratory Data Analysis (EDA)
EDA is performed to understand the data distribution, relationships between variables, and identify potential patterns or anomalies. This includes:

Visualizing missing data using missingno.
Displaying descriptive statistics for various subsets of the data.
Plotting distributions and box plots for key features (Inject_P, Leser_D).
Analyzing correlations using heatmap visualizations.
Visualizing relationships between Temperature Mean, Injection Pressure, and Microns Mean.
5. Peak Detection and Data Segmentation
This section focuses on identifying significant peaks in the 'Inject_P' (injection pressure) signal for each file. Using scipy.signal.find_peaks, the highest peak is detected within a specific time range. The data is then segmented, starting from the identified highest peak, to focus on the most relevant part of the signal for further analysis.

6. Feature Engineering and Aggregation
Raw time-series data is aggregated into a concise set of features per file. An aggregate_features function is defined to calculate statistics such as mean, standard deviation, minimum, and maximum for 'Leser_D' and 'Inject_P', along with mean values for 'Freq', 'Time', 'Temp', and 'Microns'. This aggregated DataFrame (agg_df_complete) serves as the basis for model training.

7. Model Training and Evaluation
The aggregated data is split into training, validation, and testing sets based on unique file numbers. Two types of regression models are implemented and evaluated:

Random Forest Regressor
A RandomForestRegressor is trained to predict 'Microns_mean'. Model performance is assessed using Mean Absolute Error (MAE) and R-squared. Actual vs. Predicted values are plotted to visually evaluate performance.

Gaussian Process Regressor (GPR)
Model Training: A GaussianProcessRegressor is trained using scaled data, incorporating Matern and WhiteKernel for noise. The model provides both point predictions and uncertainty estimates (epistemic and aleatoric).
Uncertainty Quantification: The notebook demonstrates how to calculate and visualize epistemic and total uncertainty, providing a measure of confidence in the predictions.
Hyperparameter Optimization: GridSearchCV is used to explore different kernel combinations for the GPR model, optimizing for MAE and R-squared. Bayesian optimization with gp_minimize from scikit-optimize is also employed to fine-tune length_scale and noise_level hyperparameters.
Posterior Sampling: The notebook illustrates sampling from the GPR's posterior distribution to generate multiple predictive curves, offering a probabilistic view of predictions and their associated uncertainties.
8. Conclusion
This notebook demonstrates a comprehensive workflow for analyzing microscopic measurement data, including:

Data Loading and Preprocessing: Handling multiple CSV files across different temperatures and frequencies, renaming, filtering, and combining them.
Feature Engineering: Aggregating raw data into meaningful features (mean, std, min, max).
Exploratory Data Analysis: Visualizing data distributions, relationships, and trends.
Model Training: Implementing and evaluating both Random Forest and Gaussian Process Regression models to predict micron values.
Uncertainty Quantification: Using Gaussian Processes to provide not just point predictions but also a measure of confidence (epistemic and aleatoric uncertainty) in those predictions.
Hyperparameter Optimization: Employing Grid Search and Bayesian Optimization to fine-tune model parameters for better performance.
9. Usage
To use this notebook:

Clone the Repository: Clone this repository to your local machine or open it in a cloud environment like Google Colab.
Upload Data: Ensure the necessary .zip files (50degreeC_1.zip, 65degreeC_1.zip, 80degreeC_1.zip, 50degreeC.zip, 65degreeC.zip, 80degreeC.zip) and Excel files (microscope_measurements_excel_1kHz.xlsx, microscope_measurements_excel.xlsx) are uploaded to the appropriate /content/ directory or their paths are updated in the notebook.
Run Cells: Execute the notebook cells sequentially. Pay attention to output messages and plots for insights into each step of the analysis.
Experiment: Feel free to modify parameters, experiment with different models, or add new analysis steps to further explore the data.
