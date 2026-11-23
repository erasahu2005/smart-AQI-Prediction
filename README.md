# Smart AQI Predictor

**Air Quality Index Prediction System using Machine Learning**

Smart AQI Predictor is a machine learning web application designed to forecast the Air Quality Index (AQI) for over 300 Indian cities up to 30 days ahead, complete with an autoregressive 7-day visual forecast trend and respiratory health advisories.

---

## 📌 Project Overview

Air pollution is a major environmental and public health concern across Indian urban centers. Traditional forecasting models often rely on real-time sensor streams which can suffer from missing values and API downtime.

**Smart AQI Predictor** overcomes these challenges by utilizing a robust, offline-capable machine learning architecture. The system leverages historical environmental data, temporal feature engineering (lag features, rolling averages, cyclical month sine/cosine transformations), and an XGBoost Regressor model to deliver fast, accurate multi-day AQI predictions.

---

## 🎯 Objectives

1. **Multi-City AQI Forecasting**: Provide accurate AQI predictions for 300+ Indian cities across 15+ states.
2. **Autoregressive Multi-Day Trend**: Generate sequential 7-day future AQI trends from any requested date.
3. **Health Advisory Integration**: Translate numerical AQI predictions into actionable health risk metrics (Cigarette Equivalent/day, Lung Stress %, Risk Score, and Advisory Recommendations).
4. **Standalone & Reliable Execution**: Operate entirely on a pre-processed static historical dataset without relying on fragile external web scrapers or live APIs.

---

## 🛠️ Technology Stack

| Layer | Technologies |
|---|---|
| **Machine Learning** | Python 3.11 · XGBoost · Scikit-Learn · Pandas · NumPy · Joblib |
| **Backend API** | FastAPI · Uvicorn · Pydantic |
| **Frontend UI** | React 18 · Vite · TailwindCSS · Recharts · Framer Motion · Lucide React |
| **Data Storage** | Processed Feature CSV (`data/aqi_features.csv`) · Pickled Encoders & Model (`models/`) |

---

## 🧠 Working & Model Architecture

```
[ User Selection: State, City, Date ]
               │
               ▼
[ FastAPI Backend Validation & Pre-computed City Lookup ]
               │
               ▼
[ Historical Feature Extraction: Lag 1, Lag 3, Lag 7, Rolling Mean ]
               │
               ▼
[ XGBoost Regressor Model Inference (aqi_production_model.pkl) ]
               │
               ▼
[ Autoregressive Loop: Recursive 7-Day Multi-Step Prediction ]
               │
               ▼
[ React Frontend: Green/Teal Interface with Gauge & Recharts Trend ]
```

### Autoregressive Prediction Flow:
1. **Request Reception**: User selects a state, city, and target date.
2. **Feature Construction**: Backend retrieves historical records for the specified city, computing:
   - `lag_1`, `lag_3`, `lag_7`: Temporal AQI history shifts.
   - `rolling_mean_7`: 7-day moving average buffer.
   - `month_sin`, `month_cos`: Cyclical monthly seasonality features.
   - `is_winter`, `is_summer`, `is_monsoon`, `crop_burning`: Domain seasonal indicators.
3. **Recursive Forecasting**: For future target dates, the model feeds its own previous output back into the feature array to recursively predict up to 7 consecutive days ahead.

---

## 📊 Dataset

- **Coverage**: 308 Indian Cities across 15+ States & Union Territories.
- **Records**: Over 410,000 processed daily historical entries.
- **Features Included**:
  - `district_encoded`: Categorical label encoding for districts/cities.
  - `pollutant_encoded`: Dominant pollutant label encoding.
  - `lag_1`, `lag_3`, `lag_7`, `rolling_mean_7`: Historical temporal memory.
  - `day`, `month`, `year`, `month_sin`, `month_cos`: Calendar and cyclical features.
  - `is_winter`, `is_summer`, `is_monsoon`, `crop_burning`: Seasonal domain flags.

---

## 📈 Machine Learning Model

- **Algorithm**: XGBoost Regressor (Extreme Gradient Boosting)
- **R² Score**: `0.8387` (83.87% variance explained)
- **Mean Absolute Error (MAE)**: `13.64` AQI units
- **Encoders**: Pickled `district_encoder.pkl` and `pollutant_encoder.pkl` for consistent categorical mapping.

---

## 🔮 Future Scope

- **Confidence Intervals**: Adding statistical confidence bounds for multi-week predictions.
- **Microclimate Station Integration**: Expanding city granularity to individual monitoring stations.
- **Mobile Native Application**: Porting frontend layout to React Native for iOS/Android.

---

## 🚀 Installation & Local Running

### Prerequisites
- Python 3.10+
- Node.js 18+ and npm

### 1. Backend Setup
```bash
# Navigate to project root
cd AQI_PREDICTION-main

# Install Python dependencies
pip install -r requirements.txt

# Start FastAPI Uvicorn server
uvicorn backend.api_server:app --reload --port 8000
```
*Backend API health check will be accessible at: `http://localhost:8000/health`*

### 2. Frontend Setup
```bash
# Open a new terminal and navigate to frontend
cd frontend

# Install Node dependencies
npm install

# Run Vite dev server
npm run dev
```
*Frontend interface will be accessible at: `http://localhost:5173`*

---

## 📸 Screenshots & Interface Design

The frontend features a clean college-project UI design with a Green, Teal, Slate, and White color palette:
- **Control Panel**: Dropdowns for state, city, and target date.
- **AQI Gauge & Category Card**: Instant color-coded AQI level (Good, Satisfactory, Moderate, Poor, Very Poor, Severe).
- **7-Day Trend Chart**: Recharts area chart rendering multi-day forecasts.
- **Health Advisory**: Real-world metrics including cigarette equivalence, lung stress index, and health risk score.

---

## 📄 License & Academic Attribution
Created as a standalone Machine Learning Capstone Project for Air Quality Index Prediction.
