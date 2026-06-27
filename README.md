Drift-Aware Explainable Ensemble Learning for Adaptive Network Intrusion Detection
PythonMLSHAPDatasetAccuracyAUC

📌 Overview
xEnsembleGuard is an intelligent Network Intrusion Detection System (IDS) that combines:

✅ All Ensemble Methods — Voting, Bagging, Boosting, Stacking combined
✅ SHAP Explainability — explains WHY each packet is flagged as attack
✅ ADWIN Concept Drift — detects when attack patterns change over time
✅ Zero-Day Detection — catches attacks never seen in training (Isolation Forest)
✅ Real-Time Capture — classifies live network packets using Scapy
🏗️ Architecture

┌─────────────────────────────────────────────────────┐
│              TWO-LEVEL ENSEMBLE                      │
│                                                     │
│  Level 0 — Base Learners (ALL ensemble types)       │
│  ① LightGBM    ② XGBoost    ③ CatBoost              │
│  ④ Random Forest  ⑤ Extra Trees                     │
│  ⑥ AdaBoost    ⑦ Gradient Boosting                  │
│                                                     │
│  Level 1 — Meta Models (4 stacking variants)        │
│  ① Stacking(LR)   ② Stacking(RF)                   │
│  ③ Stacking(MLP)  ④ Stacking(XGBoost)              │
│                                                     │
│  Final — Combined Meta ★                            │
│  Soft-Voting of ALL 4 meta-model probabilities      │
└─────────────────────────────────────────────────────┘
📊 Results — UNSW-NB15 Dataset
Model	Accuracy	F1-Score	AUC-ROC
Combined Meta ★ OURS	95.30%	96.25%	99.31%
Stacking (XGBoost)	95.05%	96.09%	99.24%
Stacking (RF)	95.14%	96.16%	99.22%
Stacking (MLP)	94.79%	95.89%	99.17%
Stacking (LR)	94.71%	95.85%	99.13%
LightGBM	94.47%	95.63%	99.11%
XGBoost	94.73%	95.84%	99.17%
Random Forest	94.21%	95.42%	99.01%
Voting (Soft)	94.53%	95.70%	99.10%
Bagging	93.64%	95.00%	98.87%
Boosting (AdaBoost)	91.61%	93.54%	98.15%
🔬 4 Novelties
Novelty 1 — SHAP Explainability
Uses TreeSHAP to explain every prediction
Global: feature importance across entire dataset
Local: why THIS specific packet was flagged
Plain-English explanation on real-time dashboard
Novelty 2 — ADWIN Concept Drift Detection
Detects when attack patterns change over time
Sliding window with statistical hypothesis testing
Triggers model retraining alert automatically
Validated: detected injected drift within 5-15 batches
Novelty 3 — Zero-Day Attack Detection
Isolation Forest for unsupervised anomaly detection
Detects attacks never seen in training data
AUC-ROC: 87% on zero-day simulation
No labelled attack data required
Novelty 4 — Real-Time Packet Capture
Live traffic capture using Scapy
Feature extraction per network flow
SHAP explanation per packet
Streamlit dashboard for visualization
🗂️ Project Structure

xEnsembleGuard/
│
├── Main.py                    # Full pipeline execution
├── data_preprocessing.py      # Load & preprocess UNSW-NB15
├── base_models.py             # LightGBM, XGBoost, CatBoost, RF
├── meta_model.py              # Stacking meta-learner (XGBoost)
├── ensemble_comparison.py     # Compare ALL 15 ensemble methods
├── shap_explainability.py     # SHAP global & local explanations
├── concept_drift.py           # ADWIN drift simulation
├── anomaly_detector.py        # Isolation Forest zero-day detection
├── packet_capture.py          # Real-time Scapy packet capture
├── live_capture_dashboard.py  # Streamlit real-time dashboard
├── evaluation.py              # Paper metrics & plots
│
├── data/                      # UNSW-NB15 train/test CSV
├── models/                    # Saved trained models (.pkl)
├── plots/                     # All generated figures
└── results/                   # CSV tables for paper
🚀 Quick Start
1. Install dependencies
bash

pip install lightgbm xgboost catboost shap scikit-learn \
            river scapy streamlit pandas numpy matplotlib seaborn
2. Download Dataset
Place UNSW-NB15 CSV files in data/ folder:

UNSW_NB15_training-set.csv
UNSW_NB15_testing-set.csv
Download from: https://research.unsw.edu.au/projects/unsw-nb15-dataset

3. Run Full Pipeline
bash

python Main.py --dataset UNSW_NB15
4. Run Ensemble Comparison Only
bash

python -c "
from data_preprocessing import DataPreprocessor
from ensemble_comparison import EnsembleComparison
data = DataPreprocessor('UNSW_NB15').load()
ec = EnsembleComparison(class_names=['Normal','Attack'])
ec.run_all(data['X_train'], data['y_train_bin'],
           data['X_test'],  data['y_test_bin'])
ec.final_report(data['X_test'], data['y_test_bin'])
"
5. Launch Real-Time Dashboard
bash

# Terminal 1 — start packet capture (needs sudo on Mac)
sudo python packet_capture.py
# Terminal 2 — launch dashboard
streamlit run live_capture_dashboard.py
📦 Dataset
Property	Value
Name	UNSW-NB15
Source	Australian Centre for Cyber Security
Records	257,673
Features	42
Attack Types	DoS, Exploits, Fuzzers, Generic, Reconnaissance, Backdoor, Analysis, Shellcode, Worms
Task	Binary Classification (Normal vs Attack)
🛠️ Tech Stack
Component	Technology
Base Models	LightGBM, XGBoost, CatBoost, Random Forest, Extra Trees
Boosting	AdaBoost, Gradient Boosting
Meta-Learner	XGBoost (Stacking)
Explainability	SHAP (TreeSHAP)
Drift Detection	ADWIN (river library)
Anomaly Detection	Isolation Forest (scikit-learn)
Packet Capture	Scapy
Dashboard	Streamlit
👨‍💻 Author
Kasanagottu Sai Maniteja
B.Tech — Computer Science & Engineering

