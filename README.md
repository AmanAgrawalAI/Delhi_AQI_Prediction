# 🌫️ Delhi PM2.5 Prediction

Predicting hourly-scale **PM2.5 concentration (µg/m³)** in Delhi from pollutant and weather readings, using a Random Forest Regressor on a dataset of **10.3M+ records** from 43 monitoring-station files.

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Pandas](https://img.shields.io/badge/Pandas-data%20processing-150458)
![Scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📌 Problem Statement

Delhi regularly faces severe air pollution, and PM2.5 is one of its most harmful pollutants. This project builds a regression model that estimates PM2.5 from other pollutant measurements (PM10, NO₂, CO, etc.), weather variables (humidity, temperature, wind) and time features.

## 📊 Dataset

| Item | Details |
|---|---|
| Files | 43 CSV files (`DL01.csv` – `DL43.csv`), one per monitoring station |
| Raw size | 10,342,500 rows × 22 columns |
| After cleaning | 10,329,521 rows (12,979 duplicate rows removed) |
| Frequency | 15-minute readings, starting January 2009 |
| Target | `pm2.5` |
| Source | [add dataset link here] |

**Features used (19):** `pm10`, `no`, `no2`, `so2`, `co`, `nox`, `ozone`, `nh3`, `benzene`, `toluene`, `rh`, `ws`, `wd`, `bp`, `at`, plus `year`, `month`, `day`, `hour` extracted from the timestamp.

## ⚙️ Methodology

1. **Merged** all 43 station files into one DataFrame.
2. **Dropped sparse columns** with more than 80% missing values: `vws`, `eth_benzene`, `mp_xylene`, `o_xylene`, `xylene`.
3. **Handled missing values** with median imputation and **removed duplicate rows**.
4. **Engineered time features** (year, month, day, hour) from the timestamp.
5. **Sampled 200,000 rows** (random, `random_state=42`) for model training because of memory limits.
6. **Split** the sample 80/20 into train and test sets.
7. **Trained and compared** a Linear Regression baseline against a Random Forest Regressor (`n_estimators=50`).
8. **Evaluated** with R² and MAE, and analysed feature importance.

```text
43 station CSVs → Merge → Drop sparse columns → Impute + de-duplicate
      → Time features → 200K sample → Train/Test split
      → Linear Regression (baseline) / Random Forest → Evaluation
```

## 📈 Results

Evaluated on the 40,000-row held-out test set:

| Model | R² Score | MAE (µg/m³) |
|---|---|---|
| Linear Regression (baseline) | 0.571 | 35.94 |
| **Random Forest Regressor** | **0.751** | **24.34** |

The Random Forest cuts the baseline's error by about 32% (MAE 35.94 → 24.34). For context, the mean PM2.5 in the raw data is roughly 104 µg/m³.

### Top features (Random Forest importance)

| Rank | Feature | Importance |
|---|---|---|
| 1 | `pm10` | 54.8% |
| 2 | `month` | 8.7% |
| 3 | `at` (air temperature) | 3.4% |
| 4 | `rh` (relative humidity) | 2.9% |
| 5 | `nh3` | 2.6% |

PM10 dominates, which is expected because it physically includes PM2.5 particles. The importance of `month` reflects Delhi's strong seasonal pattern (winter pollution peaks).

![Feature Importance]
<img width="1210" height="722" alt="Screenshot 2026-08-25 214157" src="https://github.com/user-attachments/assets/029992b5-6b0e-4d48-bfc1-255786221add" />

![Actual vs Predicted]
<img width="968" height="747" alt="Screenshot 2026-08-25 214250" src="https://github.com/user-attachments/assets/f2bddf78-d3ce-43c1-8942-3ab3aa7502a4" />


## ⚠️ Limitations

- Models were trained on a **200K-row random sample**, not the full 10.3M rows.
- A **random split** is used on time-series data, so the scores can be optimistic compared with a time-based split.
- Missing values (including some target values) were filled with the median, and station identity was not kept as a feature.
- No cross-validation or hyperparameter tuning has been done yet.

## 🔮 Future Work

- [ ] Time-based train/test split and cross-validation
- [ ] Drop rows with missing PM2.5 instead of imputing the target
- [ ] Add a `station` feature and train on a larger sample
- [ ] Hyperparameter tuning and comparison with XGBoost / LightGBM
- [ ] Deploy as a simple Streamlit app

## 🛠️ Tech Stack

Python · Pandas · NumPy · Matplotlib · Scikit-learn · Joblib · Jupyter Notebook

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/AmanAgrawalAI/Delhi_AQI_Prediction.git
cd Delhi_AQI_Prediction

# 2. Install dependencies
pip install -r requirements.txt

# 3. Place the dataset CSV files (DL01.csv ... DL43.csv) in the project folder

# 4. Open the notebook
jupyter notebook Delhi_AQI_Prediction.ipynb
```

## 📁 Project Structure

```text
Delhi_AQI_Prediction/
├── Delhi_AQI_Prediction.ipynb   # Full analysis and modelling notebook
├── images/                      # Result plots used in this README
├── requirements.txt             # Python dependencies
├── LICENSE                      # MIT License
└── README.md
```

## 👨‍💻 Author

**Aman Kumar**
Aspiring Data Scientist | Python · Machine Learning · Computer Vision

[LinkedIn](https://www.linkedin.com/in/aman-kumar-788677378) • [GitHub](https://github.com/AmanAgrawalAI) • [Email](https://mail.google.com/mail/?view=cm&fs=1&to=amanagrawal06922@gmail.com)

---
## 📄 License

This project is licensed under the **MIT License**.

⭐ If you found this project useful, consider giving it a star!
