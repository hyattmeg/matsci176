# Machine Learning-Based Screening of Li compounds for Solid State Batteries

This project provides a novel method of utilizing machine learning (ML) algorithms to assist with screening and discovery of new Li-based solid electrolyte materials for use in solid-state lithium-ion batteries (SSLIBs). 

---

## Installation

This project uses [`pymatgen`](https://pymatgen.org/) (Python Materials Genomics), an open-source Python library for materials analysis. **Pymatgen** is essential for retrieving atomic structures and calculating material properties.

To install `pymatgen`:

pip:
```sh
pip install pymatgen
```
conda:
```sh
conda install -c conda-forge pymatgen
```

For proper functionality, ensure you have an API key from [Materials Project](https://materialsproject.org/) to access structural data.

In addition, to run the code in this repository, install the required Python packages for machine learning and analysis:

pip:
```sh
pip install numpy pandas scikit-learn matplotlib seaborn xgboost pymatgen shap
```
conda:
```sh
conda install -c conda-forge numpy pandas scikit-learn matplotlib seaborn xgboost pymatgen shap
```


In addition, the `sklearn` package is used to import various machine learning functions including `LogisticRegression`, `BaggingClassifier`, `MLPClassifier`, and various classification and analysis functions. `xgboost` and `shap` are also used. These packages can be installed in the same manner as above. 

---

## Usage

### **Extract Material Features**
Run `data-extraction.ipynb` to:
- Access materials in the Materials Project Database via `pymatgen`
- Screen Li-based materials for properties suitable for use in solid electrolytes 
- Compute structural descriptors
- Save processed feature matrices as .csv files

### **Train Machine Learning Models**
Run `machine-learning.ipynb` to:
- Train logistic regression, XGBoost, bagging, and neural networks
- Evaluate models using classification metrics (F1-score, ROC-AUC)
- Identify most important features using SHAP analysis
- Apply the trained model to screen new materials

### **Data**
`feature_matrix_train.csv`:

This training data is manually extracted from the Li-based and their DFT-verified ionic conductivity values according to tables 1 and 2 in [Sendek et al. 2017, 2019](https://pubs.acs.org/doi/10.1021/acs.chemmater.8b03272). The first 5 columns consist of 5 structural features used to determine ionic conductivity, and the 6th column is the classification vector, containing a Boolean vector that distinguishes between superionic structures ($\sigma \geq 10^{-4} S/cm$) and non-superionic structures ($\sigma < 10^{-4} S/cm$).

`feature_matrix_test.csv`:

This test data contains a verifiable set of materials and their corresponding experimentally-measured ionic conductivities listed in the [Liverpool Ionics Dataset](https://pcwww.liv.ac.uk/~msd30/lmds/LiIonDatabase.html). Filtering through this dataset for materials that also exist in the MPD provided us with a test set containing 28 elements. Our feature matrix is a compilation of the 5 structural features described above for these 28 elements, and our classification vector used to determine performance metrics for each of our ML models is a Boolean vector that distinguishes between superionic structures ($\sigma \geq 10^{-4} S/cm$) and non-superionic structures ($\sigma < 10^{-4} S/cm$) listed in the Liverpool Ionics Database.

`feature_matrix_sample.csv`:

This file contains the resulting 5 structural features and superionicity classifications resulting from the extraction and filtering processes in `data-extraction.ipynb`. The 20,000+ Li-based materials in the Materials Project Database can be pre-filtered based on the following parameters:

1. **Band gap**: Materials with a band gap lower than 1 eV can conduct electrons too easily and thus would not be effective ion conductors.
2. **Energy above hull**: Materials need to be thermodynamically stable to operate as an effective ion conductor, so $E_{hull}$ should equal 0 eV.
3. **Contains transition metals**: Previous studies have shown that Li-based materials without transition metals typically do not operate as effective ion conductors.

Since these materials do not have verified ionic conductivity values, these values were used for new material prediction. 

`Li_sample_predictions.csv`: 

This file contains all parameters in feature_matrix_sample, with the last column containing the results of our predictions. 

---

## Contact

For questions or collaborations, feel free to reach out:

- **Aarya Bawishi** - aarya19@stanford.edu
- **Megan Hyatt** - hyattmeg@stanford.edu
