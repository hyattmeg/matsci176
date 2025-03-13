# Machine Learning-Based Screening of Li compounds for Solid State Batteries

This project provides a novel method of utilizing machine learning (ML) algorithms to assist with screening and discovery of new Li-based solid electrolyte materials for use in solid-state lithium-ion batteries (SSLIBs). The code in this project trains multiple ML models, including logistic regression, bagging, XGBoost, and neural networks, to classify lithium-containing materials as superionic or non-superionic. These classifications are verified using measured ionic conductivity values and show that advanced ML techniques outperform traditional linear models, with neural networks achieving the highest predictive accuracy (79\%) and recall (70\%) for superionic materials. SHAP analysis further reveals the importance of Li–anion separation distance and anion coordination features. Furthermore, after pre-screening 21,000+ Li-based compounds using structural descriptors derived from Density Functional Theory (DFT) calculations, we apply Neural Networks, our best-performing model, identify promising candidates with high predicted stability and conductivity. Among the predicted candidates, compounds like $Li_{12}Be_6F_{24}$, $Li_{12}P_{12}O_{36}$, and $Li_3B_3F_{12}$ stand out as promising compounds for application in SSLIBs.

---

## Installation

This project uses [Pymatgen](https://pymatgen.org/) (Python Materials Genomics), an open-source Python library for materials analysis. `Pymatgen` is essential for retrieving atomic structures from the [Materials Project Database (MPD)](https://materialsproject.org/).

To install `pymatgen`:

pip:
```sh
pip install pymatgen
```
conda:
```sh
conda install -c conda-forge pymatgen
```

In addition, to run the code in this repository, install the required Python packages for machine learning and analysis:

pip:
```sh
pip install numpy pandas scikit-learn matplotlib seaborn xgboost pymatgen shap
```
conda:
```sh
conda install -c conda-forge numpy pandas scikit-learn matplotlib seaborn xgboost pymatgen shap
```
---

## Usage

### **Extract Material Features**
Use `data-extraction.ipynb` to:
- Accesses materials in the Materials Project Database via `pymatgen`
- Screens Li-based materials for properties suitable for use in solid electrolytes 
- Computes structural descriptors
- Saves processed feature matrices as .csv files

For proper functionality of this code, ensure you have an API key from the MPD to access structural data. You will need to change the following variable in the code to match your pesonal API key: 

```sh
API_KEY = "INSERT YOUR API KEY HERE"
```

Variables within the `mpr.materials.summary.search` function can be modified to change which materials are extracted from the MPD. See the [MPD API](https://api.materialsproject.org/docs#/) website for descriptions of each function. 

### **Train Machine Learning Models**
Use `machine-learning.ipynb` to:
- Trains logistic regression, XGBoost, bagging, and neural networks
- Evaluates models using classification metrics (F1-score, ROC-AUC)
- Identifies most important features using SHAP analysis
- Applies the trained model to screen new materials

Datasets can be downloaded from this project, or new datasets with the same column structure can be uploaded. The following lines of code will need to be modifed to import datasets on your personal device:

```sh
df = pd.read_csv("feature_matrix_train.csv")
df_test = pd.read_csv("feature_matrix_test.csv")
```

## Data

### **Train Set**
`feature_matrix_train.csv`:

This training data is manually extracted from the Li-based and their DFT-verified ionic conductivity values according to tables 1 and 2 in [Sendek et al. 2017, 2019](https://pubs.acs.org/doi/10.1021/acs.chemmater.8b03272). The first 5 columns consist of 5 structural features used to determine ionic conductivity, and the 6th column is the classification vector, containing a Boolean vector that distinguishes between superionic structures ($\sigma \geq 10^{-4} S/cm$) and non-superionic structures ($\sigma < 10^{-4} S/cm$).

### **Test Set**
`feature_matrix_test.csv`:

This test data contains a verifiable set of materials and their corresponding experimentally-measured ionic conductivities listed in the [Liverpool Ionics Dataset](https://pcwww.liv.ac.uk/~msd30/lmds/LiIonDatabase.html). Filtering through this dataset for materials that also exist in the MPD provided us with a test set containing 28 elements. Our feature matrix is a compilation of the 5 structural features described above for these 28 elements, and our classification vector used to determine performance metrics for each of our ML models is a Boolean vector that distinguishes between superionic structures ($\sigma \geq 10^{-4} S/cm$) and non-superionic structures ($\sigma < 10^{-4} S/cm$) listed in the Liverpool Ionics Database.

### **Predictive Sample Set**
`feature_matrix_sample.csv`:

This file contains the resulting 5 structural features and superionicity classifications resulting from the extraction and filtering processes in `data-extraction.ipynb`. The 21,000+ Li-based materials in the Materials Project Database can be pre-filtered based on the following parameters:

1. **Band gap**: Materials with a band gap lower than 1 eV can conduct electrons too easily and thus would not be effective ion conductors.
2. **Energy above hull**: Materials need to be thermodynamically stable to operate as an effective ion conductor, so $E_{hull}$ should equal 0 eV.
3. **Contains transition metals**: Previous studies have shown that Li-based materials without transition metals typically do not operate as effective ion conductors.

Since these materials do not have verified ionic conductivity values, these values were used for new material prediction. 

### **Superionicity Prediction Results**
`Li_sample_predictions.csv`: 

This file contains all parameters in feature_matrix_sample, with the last column containing the results of our predictions. 

---

## Contact

For questions or collaborations, feel free to reach out:

- **Aarya Bawishi** - aarya19@stanford.edu
- **Megan Hyatt** - hyattmeg@stanford.edu
