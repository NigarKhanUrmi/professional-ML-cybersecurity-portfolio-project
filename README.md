# professional-ML-cybersecurity-portfolio-project
Machine learning-based DDoS attack detection using CIC-DDoS2019, comparing six classifiers with all features and Pearson-selected 22 features to evaluate detection performance and computational efficiency.

**Machine Learning-Based DDoS Attack Detection: Feature Selection and Computational Efficiency**

Distributed Denial-of-Service (DDoS) attacks send a large amount of traffic to network resources and can make online services unavailable to legitimate users. In this project, I studied whether machine-learning models can identify the difference between benign network traffic and DDoS attacks while using fewer input features and reducing the computational cost.
**Research Question:** The primary research question is: How does Pearson correlation-based feature selection affect the classification performance and computational efficiency of six machine-learning models in distinguishing benign network traffic from DDoS attacks using the CIC-DDoS2019 dataset?
**Objectives:** The main objective of this project is to differentiate between DDoS attacks and benign network traffic by analyzing the importance of network traffic features and evaluating how feature selection affects classification performance and computational efficiency.
**Sub-objectives:**
1.	Establish a reliable classical machine-learning baseline for distinguishing DDoS attacks from benign network traffic.
2.	Compare the classification performance of six machine-learning models: Decision Tree, Random Forest, AdaBoost, XGBoost, MLP, and DNN.
3.	Evaluate the impact of Pearson correlation-based feature reduction on model performance and computational efficiency, while following selected methods from Becerra-Suarez et al. (2024) using the CIC-DDoS2019 dataset.

**Dataset**

I used the publicly available CIC-DDoS2019 dataset(https://data.mendeley.com/datasets/ssnc74xm6r/1) from Mendeley Data. The dataset contains network-flow 400K+ records with approximately 80 features describing traffic characteristics, including flow duration, packet counts, packet lengths, and traffic rates. The original dataset version described in the reference paper contains 431,371 records, including benign and DDoS traffic.
To keep the experiment computationally manageable, I selected 20,000 records, with 10,000 benign and 10,000 DDoS records. The prediction target has to be binary: benign traffic or DDoS attack. The sample size, class distribution, data quality, and available features were verified during exploratory data analysis.
***Due to dataset size and redistribution considerations,the raw dataset is not included in this repository.***

The project compared six machine-learning models using two feature sets:

- All 66 eligible features
- 22 Pearson-selected features

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Training time
- Prediction time

The final results showed how much performance was retained after reducing the feature space from 66 to 22 features and whether the reduced feature
set improved computational efficiency.

**Methodology**

After the data collection, I examined the data for missing values, infinite values, duplicate records, class imbalance, constant features, and irrelevant columns. I removed duplicate records, handled missing and infinite values, and dropped columns such as record IDs, flow identifiers, and redundant labels that could affect the model's performance or cause data leakage. I also separated the target label from the input features before training the models. As part of exploratory data analysis, I applied K-Means clustering to the cleaned training data without using the target labels. This unsupervised analysis helped to identify whether the network traffic naturally forms distinct groups. I used PCA to visualize the resulting clusters in two dimensions and compare the cluster composition with the known Benign and DDoS labels for interpretation. However, the clustering results were not treated as proof that a cluster represents a particular traffic class. K-Means only identifies groups based on feature similarity and does not establish reliable DDoS classification. Therefore, supervised classification is still required for detection.
Then I compared two approaches: using all the cleaned features and using a smaller set of 22 features selected through Pearson correlation. I evaluated six machine learning models: Decision Tree, Random Forest, AdaBoost, XGBoost, Multilayer Perceptron (MLP), and Deep Neural Network (DNN). Decision Tree provided a simple baseline, while the other models helped us to compare different ensemble-learning and neural-network approaches. 
I split the data into training and testing sets, keeping the test set separate for the final evaluation. I used stratified five-fold cross-validation on the training data to evaluate model performance and assess the robustness of the selected model configurations. Also, because the dataset is a historical benchmark dataset rather than a continuously collected deployment dataset, the test set represents unseen records from the same benchmark population rather than future real-world network traffic.
To avoid data leakage, all preprocessing steps and feature selection were performed using training data only. Finally, I compared the two feature sets using accuracy, precision, recall, F1-score, validation ROC – AUC, confusion matrix, training time, and prediction time. This helped us to determine whether using only 22 features can maintain good DDoS detection performance while lowering the computational cost.

A stratified 5-fold cross-validation procedure was applied to the training data. Within each fold, missing-value imputation and feature scaling were fitted using only the fold's training portion. For the Pearson feature-selection experiment, the 22 features were selected using only the training portion of each fold before evaluating the corresponding validation fold.

**Exploratory Data Analysis (EDA)**

I conducted exploratory data analysis to understand the dataset's structure, feature distributions, and data quality before model training. I examined the class distribution, missing and infinite values, duplicate records, constant features, and potential identifier or label-leakage columns. Histograms and box plots were used to examine feature distributions and identify potential outliers. A correlation heatmap helped us to identify relationships between numerical features and possible redundancy among predictors. I also examined the absolute Pearson correlations between individual features and the target variable to guide the selection of 22 features.
The EDA showed substantial differences in the distributions of several network-flow features and some highly correlated predictors. These observations supported the use of feature scaling and motivated the Pearson-based feature reduction experiment.


**Feature Selection**

After splitting the dataset into training and testing sets, Pearson correlation was used to evaluate the relationship between the input features and the target variable. The 22 features with the strongest absolute Pearson correlation with the target were selected for the reduced-feature experiments. The same selected feature set was used across all six classification models—Decision Tree, Random Forest, AdaBoost, MLP, DNN, and XGBoost—to ensure a consistent comparison between the full-feature and reduced-feature approaches. The test set was not used during feature selection to prevent data leakage.
 
**Baseline Approach: All Features**

The baseline approach used all eligible numerical features remaining after data cleaning and the removal of target labels, redundant columns, identifiers, and constant features. I trained and evaluated six classification models: Decision Tree, Random Forest, AdaBoost, XGBoost, Multilayer Perceptron (MLP), and a deeper neural-network model (DNN) with all the features
(66 features: excluding target labels and null columns). This approach established the reference performance and computational cost for DDoS attack detection without Pearson-based feature reduction.


**Experimental Approach: Pearson-Selected 22 Features**

In the experimental approach, I used Pearson correlation to select the 22 features most strongly associated with the target variable, based on absolute correlation values. I trained and evaluated the same six classifiers using this reduced feature set. This allowed us to investigate whether reducing the number of input features can maintain classification performance while reducing training time and prediction cost.
**Comparison and Evaluation**

Both approaches used the same training and testing split, evaluation metrics, and model-comparison procedure. I used stratified five-fold cross-validation on the training data to compare model performance and select configurations. Preprocessing and Pearson feature selection were performed within each cross-validation training fold to prevent data leakage. After model selection, I evaluated the chosen configurations on the held-out test set.
I compared accuracy, precision, recall, F1-score, ROC-AUC, confusion matrices, training time, prediction time, and the number of features used. The results show whether Pearson-based feature reduction provides a useful trade-off between detection performance and computational efficiency.

**Experiment**

I maintained an experiment log to record the settings and results of each model run. The log included the model name, feature set, number of features, model parameters, cross-validation results, evaluation metrics, training time, and prediction time. We documented results for all six classifiers using both the full feature set and the 22 Pearson-selected features. This log supported reproducibility and helped us to compare classification performance and computational efficiency.
 
**Published-Method Comparison**

Our reference is Becerra-Suarez, Fernández-Roman, and Forero (2024), “Improvement of Distributed Denial of Service Attack Detection through Machine Learning and Data Processing,” Mathematics, 12(9), 1294. We implemented a fair, appropriately scoped comparison based on its preprocessing, correlation-based feature reduction, and model-comparison approach. Because our sample size and selected feature count differ from the paper, published scores were treated as context rather than directly equivalent results.



Reference:
Becerra-Suarez, F. L., Fernández-Roman, I., & Forero, M. G. (2024). Improvement of Distributed Denial of Service Attack Detection through Machine Learning and Data Processing. Mathematics, 12(9), 1294. https://doi.org/10.3390/math12091294
ChatGPT (OpenAI) assisted with organization, drafting, and revision of this document.
