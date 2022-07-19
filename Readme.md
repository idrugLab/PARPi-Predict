# **Environment**

The most important python packages are:
- python == 3.7.10
- torch == 1.9.0+cu111
- tensorboard == 2.5.0
- rdkit == 2021.09.3
- scikit-learn == 1.0
- hyperopt == 0.2.7
- numpy == 1.19.5

For using our model more conveniently, we provide the environment file *<environment.txt>* to install environment directly.


---
# **PARPs Pred**
### 1. Data
Data file is in ***dataset/***

### 2. Split data
Split data file is in ***dataset_split/***

### 2. Trained Model
The best trained model is ***trained_model/model.pt***

### 3. Prediction with trained models
For example, to predict new molecules (in *test.csv*) with trained PARPs model, use the command:

`python predict.py  --predict_path test.csv  --model_path trained_model/model.pt  --result_path result_PARPs.csv`

The result file contains the prediction of PARPs. 


---
# **Web Sever**

​A web-based online platform called PARPi-Predict (https://parpipredict.idruglab.cn/) have been developed:

### **1.**
draw a structure online, input SMILES or upload structures filer

The format of the input file:
1. The format of the input file should be csv.
2. In the input file, the contents are as follows:
```
SMILES
O(C(=O)C(=O)NCC(OC)=O)C
FC1=CNC(=O)NC1=O
...
```

### **2.**
choose the target(s) (ALL, PARP-1, PARP-2, PARP-5A, PARP-5B)

### **3.**
The predicted molecule profiling results by PARPi-Predict can be downloaded as .csv

### **4.**
The result file contains the prediction of four PARP subtypes.

| # | SMILES | PARP-1 | PARP-2 | PARP-5A | PARP-5B |
| ----- | ----- | ----- | ----- | ----- | ----- |
| 1 | CCC(CC)COC(=O)\[C@H](C)N\[P@](=O)(OC\[C@H]1O\[C@@](C#N)(c2ccc3c(N)ncnn23)\[C@H](O)\[C@@H]1O)Oc1ccccc1	| 0.997	| 0.999	| 1.000	| 1.000 |
| 2 | \[H]c1c(\[H])c(\[H])c(C(=O)C(\[H])(\[H])Oc2c(\[H])c(\[H])c(\[H])c3c(=O)n(\[H])c(\[H])c(\[H])c23)c(\[H])c1\[H] | 0.498 | 0.323 | 0.122	| 0.246 |
| 3 | FC1=CC=C(CC2=NNC(=O)C3=CC=CC=C23)C=C1C(=O)N1CCN(CC1)C(=O)C1CC1 | 1.000 | 0.999 | 0.496 | 0.415 |
| 4 | NC(Cn1ccc(=O)n(Cc2ccccc2C(=O)O)c1=O)C(=O)O | 0.991 | 0.996	| 0.971 | 0.788 |
| 5 | Cc1n\[nH]c(C)c1N=Nc1cc(Cl)ccc1Cl	| 0.965	| 0.940	| 0.728	| 0.437 |

---
# **Commands of FP-GNN**

### **1. Train**
Use train.py

Args:
  - data_path : The path of input CSV file. *E.g. input.csv*
  - dataset_type : The type of dataset. *E.g. classification  or  regression*
  - save_path : The path to save output model. *E.g. model_save*
  - log_path : The path to record and save the result of training. *E.g. log*

E.g.

`python train.py  --data_path data/test.csv  --dataset_type classification  --save_path model_save  --log_path log`

### **2. Predict**
Use predict.py

Args:
  - predict_path : The path of input CSV file to predict. *E.g. input.csv*
  - result_path : The path of output CSV file. *E.g. output.csv*
  - model_path : The path of trained model. *E.g. model_save/model.pt*

E.g.

`python predict.py  --predict_path data/test.csv  --model_path model_save/test.pt  --result_path result.csv`

### **3. Hyperparameters Optimization**
Use hyper_opti.py

Args:
  - data_path : The path of input CSV file. *E.g. input.csv*
  - dataset_type : The type of dataset. *E.g. classification  or  regression*
  - save_path : The path to save output model. *E.g. model_save*
  - log_path : The path to record and save the result of hyperparameters optimization. *E.g. log*

E.g.

`python hyper_opti.py  --data_path data/test.csv  --dataset_type classification  --save_path model_save  --log_path log`

### **4. Interpretation of Fingerprints**
Use interpretation_fp.py

Args:
  - predict_path : The path of input CSV file. *E.g. input.csv*
  - model_path : The path of trained model. *E.g. model_save/model.pt*
  - result_path : The path of result. *E.g. result.txt*

E.g.

`python interpretation_fp.py  --predict_path test.csv  --model_path model_save/test.pt  --result_path result.txt`

### **5. Interpretation of Graph**
Use interpretation_graph.py

Args:
  - predict_path : The path of input CSV file. *E.g. input.csv*
  - model_path : The path of trained model. *E.g. model_save/model.pt*
  - figure_path : The path to save figures of graph interpretation. *E.g. figure*

E.g.

`python interpretation_graph.py  --predict_path test.csv  --model_path model_save/test.pt  --figure_path figure`


---
# **Data Standard of FP-GNN**

We provide the three public benchmark datasets used in our study: *<Data.rar>*

Or you can use your own dataset:
### 1. For training
The dataset file should be a **CSV** file with a header line and label columns. E.g.
```
SMILES,PARP-1
O(C(=O)C(=O)NCC(OC)=O)C,0
FC1=CNC(=O)NC1=O,0
...
```
### 2. For predicting
The dataset file should be a **CSV** file with a header line and without label columns. E.g.
```
SMILES
O(C(=O)C(=O)NCC(OC)=O)C
FC1=CNC(=O)NC1=O
...
```
### 3. For interpreting fingerprints
The dataset file should be a **CSV** file with a header line and label columns. E.g.
```
SMILES,PARP-1
O(C(=O)C(=O)NCC(OC)=O)C,0
FC1=CNC(=O)NC1=O,0
...
```
### 4. For interpreting molecular graphs
The dataset file should be a **CSV** file with a header line and without label columns. E.g.
```
SMILES
O(C(=O)C(=O)NCC(OC)=O)C
FC1=CNC(=O)NC1=O
...
```

