# MetaPaCS: A Novel <ins>Meta</ins>-learning Model for <ins>Pa</ins>ncreatic <ins>C</ins>ancer <ins>S</ins>ubtype Prediction 
**MetaPaCS** (an Ensemble stacking-Based Model for Identifying Pancreatic Cancer Subtypes), is an accurate and cost-effective model for Pancreatic Cancer subtype prediction based on RNA-seq Expression data only. Leveraging multiple different machine learning techniques, MetaPaCS is able to identify accurately and efficiently predict 4 different Pcancreatic Cancer subtypes, which may provide insights into the characteristics of these subtypes that can significantly aid clinical decision-making processes.

## Flowchart of MetaPaCS
![Flowchart of MetaPaCS](flowchart.png)

## Table of Contents
- [Installation](#installation)
- [Tutorials](#Tutorials)
- [Bug Report](#Bug-Report)
- [Authors](#Authors)
- [Publication](#Publication)

## Tutorials
### Jupyter notebook
1. Download MetaPaCS and LIMMAdata.csv from the github
2. Open the MetaPaCS code in jupyter notebook
3. Specify the current directory, put LIMMAdata.csv or your input data in this directory

<pre> ```from xgboost import XGBClassifier

os.chdir("") #set to your working directory

warnings.filterwarnings("ignore", category=UserWarning) ``` </pre>


4. Put LIMMAdata.csv (test data) or your own input data (must be structured as below) in this directory and specify the desired output directory

<pre> ```INPUT_CSV = "./ICGCfilterREDO.csv"
OUTPUT_ROOT = "./output" #change to your desired output name ``` </pre>



5. enable the desired classifier algorithms in the base and stacking catalog

<pre> ```def build_model_catalog(seed: int) -> Tuple[List[Tuple[str, object]], List[Tuple[str, object]]]:
    base_catalog = [
        ("svm_rbf", SVC(kernel="rbf", probability=True, break_ties=True, random_state=1, C=1, gamma="scale", class_weight=None)),
        ("svm_linear", SVC(kernel="linear", probability=True, break_ties=True, random_state=1, C=0.00075, class_weight="balanced")),
        ("lr", LogisticRegression(max_iter=700, solver="lbfgs")),
        ("rf", RandomForestClassifier(n_estimators=100, random_state=seed, n_jobs=1)),
        ("xgb", XGBClassifier(n_estimators=300, eval_metric="logloss", random_state=seed, subsample=1.0, colsample_bytree=1.0, n_jobs=1)),
        ("knn", KNeighborsClassifier(n_neighbors=5, weights="distance")),
        ("qda", QuadraticDiscriminantAnalysis(reg_param=1.0, store_covariance=True, tol=0.0)),
        ("dt", DecisionTreeClassifier(random_state=SEED)),
        ("mlp", MLPClassifier(activation="relu", alpha=0.0001, learning_rate="constant", max_iter=210, batch_size=8, solver="adam", hidden_layer_sizes=(100, 100), random_state=SEED, shuffle=True)),
        ("nb", GaussianNB())
    ]

    stack_catalog = [
        ("stack_xgb", XGBClassifier(n_estimators=300, eval_metric="logloss", random_state=seed, subsample=1.0, colsample_bytree=1.0, n_jobs=1)),
        ("stack_qda", QuadraticDiscriminantAnalysis(reg_param=1.0, store_covariance=True, tol=0.0)),
        ("stack_knn", KNeighborsClassifier(n_neighbors=5, weights="distance")),
        ("stack_rf", RandomForestClassifier(n_estimators=100, random_state=seed, n_jobs=1)),
        ("stack_lr", LogisticRegression(max_iter=700, solver="lbfgs")),
        ("stack_svm_rbf", SVC(kernel="rbf", probability=True, break_ties=True, random_state=1, C=1.5, gamma="scale", class_weight="balanced")),
        ("stack_svm_linear", SVC(kernel="linear", probability=True, break_ties=True, random_state=1)),
        ("stack_dt", DecisionTreeClassifier(random_state=SEED)),
        ("stack_mlp", MLPClassifier(activation="relu", alpha=0.0001, learning_rate="constant", max_iter=210, batch_size=8, solver="adam", hidden_layer_sizes=(100, 100), random_state=SEED, shuffle=True)),
        ("stack_nb", GaussianNB())
    ]
    return base_catalog, stack_catalog ``` </pre>

6. set the combination testing number to the same number as the amount of classifiers or to the desired size of combinations to be tested

   <pre> ```def generate_base_combinations(base_catalog: List[Tuple[str, object]]) -> List[Tuple[List[str], List[object]]]:
    all_combos: List[Tuple[List[str], List[object]]] = []
    indices = list(range(len(base_catalog)))

    #if only using a single configuration, set the range to (number of base classifiers, number of base classifiers +1)
    #if testing combinations, set the number to the desired amount of classifiers within the combination (ex. range(2, 3) will test all combinations of 2 classifier)
    for r in range(10, 11):  ``` </pre>


## Example Output

The prediction results and evaluations will be stored and exported into a folder in your specified directory.
Evaluations are saved for all base and meta-learning classifiers.

''''''

![Example Outputs](OUTPUTgithub.png)

## Bug Report

If you find any bugs or problems, or you have any comments on S, please don't hesitate to contact via email nickpeterson@unmc.edu or [Issues](https://github.com/wan-mlab/MetaPaCS/issues).

## Authors
Mengtao Sun, Nick Peterson, Shibiao Wan, Xinchao Wu

## Publication
MetaPaCS: A novel meta-learning model for pancreatic cancer subtype prediction
Nick Peterson, Mengtao Sun, Xinchao Wu, Jieqiong Wang, Shibiao Wan
bioRxiv TBD; doi: TBD

## License 

[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

GNU GENERAL PUBLIC LICENSE  
Version 3, 29 June 2007

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
