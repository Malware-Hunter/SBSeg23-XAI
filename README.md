## **Android Malware Detection using XAI Techniques**

### **Introduction**

The exponential rise in Android devices also brings with it an increased risk of malicious applications threatening users' security and privacy. This has led to the adoption of various machine learning-based methods for detecting malware on Android devices. However, a significant challenge arises due to the "black-box" nature of these methods, which often obscure their decision-making processes. The need for transparency, or explainability, is paramount to understand why an application is classified as benign or malicious.

**Explainable Artificial Intelligence (XAI)** techniques are pivotal in this scenario. These methods elucidate the significant features used by ML models, giving insights into how changes in those features can impact predictions. By implementing XAI techniques for Android malware detection, our aim is to improve explainability without compromising performance.

### **Objective**

In our research, we delve deep into the XAI techniques suitable for Android malware detection models. By utilizing the SHAP framework, we aim to decode what these ML models grasp during their training and unravel the contributing factors for their performance in experimental setups.


### **Code Breakdown**

**1. Data Loading and Preprocessing**
    
   After reading the data, column names are processed to ensure consistent naming conventions, removing special characters, replacing spaces with underscores, and converting them to lowercase.
data = pd.read_csv(join(paths['binaries'], files['filename'][5])) column_names = process_column(column_names=data.columns.values, delimiter='.') data.columns = column_names
````
```
data = pd.read_csv(join(paths['binaries'], files['filename'][5]))
column_names = process_column(column_names=data.columns.values, delimiter='.')
data.columns = column_names
```
````

**2. Dataset Split**

The data is split into training and validation sets. This ensures that we can train the model on one subset and validate its performance on another unseen subset.
````
```
X_train, X_val, y_train, y_val = train_test_split(X, y, test_size=0.2, random_state=0, shuffle=True)
```
````

**3. Model Training**

The training function trains an XGBoost model on the provided training data and returns predictions, confusion matrix, and a classification report for the validation data.

````
```
model, y_pred, cm, report = train(X_train.values, y_train, X_val.values, y_val, class_names=class_names, classifier='xgboost')
```
````

**4. Visualization**

Multiple visualizations are available, from confusion matrices to SHAP plots, highlighting feature importance and understanding the model's decision-making process.
````
```
plot_cm(cm, class_names)
feat_importances = plot_importance(model, feature_names=column_names)
```
````

## Datasets

To achieve a comprehensive analysis, we've incorporated multiple Android malware datasets. These datasets comprise various features representing different facets of the Android OS like permissions, system calls, hardware access, and API calls. Such diverse features are paramount to build formidable malware detection systems.

-   **Drebin-215** \cite{yerima2019droidfusion}
-   **Androcrawl** \cite{sisto2013androcrawl}
-   **KronoDroid** \cite{guerra2021kronodroid}
-   **Android-Permissions** \cite{sarma2012android}
-   **Adroit** \cite{martin2016adroit}
-   **DefenseDroid** \cite{colaco2021defensedroid}
-   **MH-100K** \cite{braganca2023capturing}

|         Dataset          | Features | Malwares | Benigns | Total |
|:------------------------:|:--------:|:--------:|:-------:|:-----:|
|       AndroCrawl         |    141   |   10170  |  86574  | 96744 |
|         ADROIT           |    166   |    3418  |   8058  | 11476 |
|   Android Permissions    |    151   |   17787  |   9077  | 26864 |
|  DefenseDroid PRS[^PRS]  |   2877   |   6000   |   5975  | 11975 |
|       DREBIN-215         |   215    |   5555   |   9476  | 15031 |
|  KronoDroid Real Device  |   286    |   41382  |  36755  | 78137 |
|    KronoDroid Emulator   |   276    |   28745  |  35246  | 63991 |

[^PRS]: Permissions, Receivers and Services 

MH-100K: [https://github.com/Malware-Hunter/MH-100K-dataset](https://github.com/Malware-Hunter/MH-100K-dataset)

These datasets encapsulate varied features crucial for Android malware detection, reflecting different facets of the Android OS like permissions, system calls, hardware interactions, and API calls.

## Findings

We observed that there's no direct overlap of feature sets across the different datasets. This indicates the extensive scope of Android malware operations, suggesting the necessity for a more comprehensive feature set to ensure accurate detection.

Our research also highlights the importance of employing multiple datasets and features for precision in identifying malware. Another key finding is the potential benefits of integrating expert-dependent features in the malware detection process. Such features can elevate model accuracy by catching subtle malicious behavior signs that might be overlooked by ML algorithms.

## Contributions

Our research contributions are dual-pronged:

1.  **XAI-based Feature Importance Analysis:** Our findings throw light on the pivotal features that AI/ML models prioritize when differentiating between benign and malicious applications. This insight augments the models' comprehensibility and interpretability.
    
2.  **XAI Integration:** Integrating XAI with malware detection models is essential for promoting top-tier research. This amalgamation aims at maintaining high accuracy while demystifying the models' decision-making processes.
    

## Repository Content

### Feature Importance

Using the SHAP framework, we extracted and analyzed feature importances for the models trained. The feature importance is critical in understanding the rationale behind model predictions. You can see the related Python functions:

-   [`get_feature_importances`](https://chat.openai.com/#get-feature-importances)
-   [`shap_explainer`](https://chat.openai.com/#shap-explainer)
-   [`plot_importance`](https://chat.openai.com/#plot-importance)

### Column Processing

For better visualization and consistency, column names across different datasets were processed and standardized. The related function for this can be found under:

-   [`process_column`](https://chat.openai.com/#process-column)

## Further Reading

The complete breakdown of our research, methodology, experimental protocols, and results are detailed in the main paper. We recommend referring to the related sections for a comprehensive understanding:

-   **Related Works:** `Section \ref{sec_related}`
-   **XAI Methodology:** `Section \ref{sec_xai}`
-   **Experimental Protocol and Results:** `Sections \ref{sec_experimental} and \ref{sec_conclusao}`
