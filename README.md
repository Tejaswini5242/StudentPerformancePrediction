# Student Performance Prediction

A machine learning web application that predicts a student's **math score** based on demographic and academic background factors. Built with a modular ML pipeline and served through a Flask web interface.

---

## What It Does

The model takes the following inputs from the user:

| Input | Type |
|---|---|
| Gender | Male / Female |
| Race / Ethnicity | Group A – E |
| Parental Level of Education | High School → Master's Degree |
| Lunch Type | Standard / Free or Reduced |
| Test Preparation Course | Completed / None |
| Reading Score | 0 – 100 |
| Writing Score | 0 – 100 |

And predicts → **Math Score (0 – 100)**

---

## How It Works

The project is split into two flows:

**Training Flow** — runs once to generate the model
```
Raw CSV → Data Ingestion → Data Transformation → Model Trainer → Model.pkl + Preprocessor.pkl
```

**Prediction Flow** — runs on every form submission
```
User Form → app.py → Prediction Pipeline → load artifacts → transform input → predict → show result
```

The `Preprocessor.pkl` ensures user input at prediction time is transformed the exact same way the training data was — same scaling, same encoding.

Six regression models are trained and compared using GridSearchCV hyperparameter tuning. The best performing model (by R² score on test data) is automatically selected and saved.

| Model |
|---|
| Linear Regression |
| Decision Tree |
| Random Forest |
| Gradient Boosting |
| CatBoost Regressor |
| AdaBoost Regressor |

---

## Project Structure

```
Student-Performance-Prediction/
│
├── app.py                          ← Flask web app, entry point
├── setup.py                        ← makes src/ an installable package
├── requirements.txt                ← all project dependencies
├── README.md
├── LICENSE
├── .gitignore
│
├── src/                            ← all ML source code
│   ├── components/
│   │   ├── Data_ingestion.py       ← reads CSV, splits into train/test
│   │   ├── Data_transformation.py  ← feature scaling and encoding
│   │   └── Model_trainer.py        ← trains, evaluates, and saves best model
│   ├── pipeline/
│   │   ├── Training_pipeline.py    ← orchestrates full training end-to-end
│   │   └── Prediction_pipeline.py  ← loads artifacts and runs inference
│   ├── exception.py                ← custom exception with file + line info
│   ├── logger.py                   ← timestamped log files
│   └── utils.py                    ← shared helpers (save/load pickle, evaluate models)
│
├── Artifacts/                      ← generated after training
│   ├── Model.pkl                   ← best trained model
│   ├── Preprocessor.pkl            ← fitted sklearn ColumnTransformer
│   ├── Raw_Data.csv
│   ├── Train_Data.csv
│   └── Test_Data.csv
│
├── notebooks/                      ← research and experimentation (not used at runtime)
│   ├── data/
│   │   └── stud.csv                ← original raw dataset
│   ├── Exploratory_Data_Analysis.ipynb
│   └── Model_Training.ipynb
│
├── templates/
│   └── home.html                   ← Flask HTML form + result display
│
├── static/
│   └── styles.css                  ← styling for the web interface
│
└── scripts/
    └── template.py                 ← one-time project scaffolding script
```

---

## Tech Stack

| Layer | Tools |
|---|---|
| Data Processing | Pandas, NumPy |
| ML & Preprocessing | Scikit-learn, CatBoost, XGBoost |
| Serialization | Pickle, Dill |
| Web Framework | Flask |
| Visualization (notebooks) | Matplotlib, Seaborn |

---

## Quick Start

```bash
# 1. Activate virtual environment
venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the app (pre-trained artifacts already included)
python app.py
```

Open `http://localhost:8080` in your browser.

> To retrain the model from scratch:
> ```bash
> python src/pipeline/Training_pipeline.py
> ```

---

## License

Distributed under the MIT License. See `LICENSE` for more information.
