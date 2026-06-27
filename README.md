# Healthcare Premium Prediction

A Streamlit web app that predicts health insurance premium costs from a person's
demographic, lifestyle, and medical details.

## Overview

The app collects user inputs (age, income, BMI, smoking status, medical history,
etc.), preprocesses them into the feature format expected by the trained models,
and returns a predicted insurance cost.

Two separate models are used depending on age:

- **`model_young`** — applied when age is **≤ 25**
- **`model_rest`** — applied when age is **> 25**

Each model has a matching scaler (`scaler_young` / `scaler_rest`) used to scale
the `age` and `income_lakhs` features before prediction.

## Project Structure

```
healthcare-premium-prediction/
├── main.py                  # Streamlit UI — collects inputs and shows the prediction
├── prediction_helper.py     # Preprocessing, scaling, and prediction logic
├── artifacts/
│   ├── model_young.joblib    # Model for ages ≤ 25
│   ├── model_rest.joblib     # Model for ages > 25
│   ├── scaler_young.joblib   # Scaler for the young model
│   └── scaler_rest.joblib    # Scaler for the rest model
├── LICENSE
└── README.md
```

## How It Works

1. **`main.py`** renders a grid of input fields (age, dependants, income,
   genetical risk, insurance plan, employment status, gender, marital status,
   BMI category, smoking status, region, and medical history) and collects them
   into a dictionary.
2. On **Predict**, the inputs are passed to `predict()` in
   **`prediction_helper.py`**, which:
   - One-hot encodes the categorical inputs into the expected feature columns.
   - Computes a **normalized risk score** from the medical history (weighted by
     disease severity — e.g. heart disease = 8, diabetes = 6).
   - Scales `age` and `income_lakhs` using the age-appropriate scaler.
   - Selects the age-appropriate model and returns the predicted cost.

### Input Features

| Feature | Type | Values / Range |
|---|---|---|
| Age | Numeric | 18–100 |
| Number of Dependants | Numeric | 0–20 |
| Income in Lakhs | Numeric | 0–200 |
| Genetical Risk | Numeric | 0–5 |
| Insurance Plan | Categorical | Bronze, Silver, Gold |
| Employment Status | Categorical | Salaried, Self-Employed, Freelancer |
| Gender | Categorical | Male, Female |
| Marital Status | Categorical | Unmarried, Married |
| BMI Category | Categorical | Normal, Obesity, Overweight, Underweight |
| Smoking Status | Categorical | No Smoking, Regular, Occasional |
| Region | Categorical | Northwest, Southeast, Northeast, Southwest |
| Medical History | Categorical | No Disease, Diabetes, High blood pressure, Thyroid, Heart disease, and combinations |

## Setup

### Requirements

- Python 3.8+
- `streamlit`
- `pandas`
- `scikit-learn`
- `joblib`

Install dependencies:

```bash
pip install streamlit pandas scikit-learn joblib
```

### Run the App

```bash
streamlit run main.py
```

Then open the URL shown in the terminal (usually http://localhost:8501) in your
browser.

## License

See [LICENSE](LICENSE).