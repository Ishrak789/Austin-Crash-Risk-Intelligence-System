# Austin-Crash-Risk-Intelligence-System

This project explores how data science and machine learning can be used to **identify when and where traffic crashes are most likely to happen in Austin, and estimate their potential severity and cost**. Using Austin’s public crash dataset along with weather, temporal, and spatial features, we built predictive models that help turn historical crash records into **actionable risk insights for public safety, city planning, and mobility decision-making**.

---

## 📌 Project Overview
- **Goal**: Predict **crash likelihood** and analyze **crash severity/cost patterns** across Austin at the **location-hour level**.  
- **Why it matters**: Traffic crashes create major public safety risks, economic losses, and operational strain for cities, insurers, and mobility platforms. Better forecasting can support **proactive interventions**, smarter resource allocation, and more targeted infrastructure planning.  
- **Approach**: Clean and transform crash-level data, enrich it with weather and time-based features, build predictive models, and evaluate how well they identify high-risk locations and conditions.  

---

## 🗂 Dataset
- **Source**: Austin Crash Report Data – Crash Level Records  
- **Raw size**: ~**224,000** crash records  
- **Cleaned size**: ~**220,000** validated crash records  
- **Features**: **47 variables** across crash, roadway, weather, time, and location data  
- **Spatial unit**: **H3 hexagons (resolution 8)**  
- **Temporal unit**: **1-hour intervals**  

### Variables Used
- **Temporal**: Hour, day of week, month, weekend/holiday flags, rush-hour indicators  
- **Spatial**: Latitude, longitude, H3 hex index, corridor/location patterns  
- **Environmental**: Rainfall, mean temperature  
- **Road context**: Speed limit, highway/freeway indicators  
- **Outcomes**: Crash occurrence, crash count, estimated crash cost  

To make the prediction setup realistic, we also created a **full location-time grid** that included both:
- periods/areas **where crashes occurred**
- periods/areas **where no crashes occurred**

This helped the model learn **true crash risk patterns**, rather than only learning from crash events themselves.

---

## ⚙️ Methods

### Preprocessing
- Cleaned crash records and removed invalid or duplicate entries  
- Aggregated crash data into **hexagon-hour bins** for spatiotemporal modeling  
- Engineered features such as **lagged crash counts**, **rolling averages**, **recency indicators**, and **weather-based signals**  
- Added zero-crash observations to correctly model a **rare-event prediction problem**  

### Models Built
1. **Binary LightGBM** ✅ (best model for crash-risk prediction)  
2. **Tweedie XGBoost** (used to estimate crash cost severity patterns)  
3. **Hurdle Model** (used to separate crash occurrence from crash count)  

### Evaluation Metrics
- ROC AUC  
- PR AUC  
- Precision  
- Recall  
- RMSE / MAE for severity-cost modeling  
- Lift over baseline for high-risk bin targeting  

---

## 📊 Results

### 1) Crash Occurrence Prediction — Binary LightGBM

| Metric | Result |
|--------|--------|
| ROC AUC | **64.76%** |
| PR AUC | **7.35%** |
| Precision | **8.47%** |
| Recall | **8.91%** |
| Baseline prevalence | **4.8%** |
| Lift in top 5% high-risk bins | **1.76x over baseline** |

➡️ The model performed well for a **rare-event prediction problem**, where crashes were extremely infrequent across the full location-time grid. Most importantly, the model’s **top 5% predicted high-risk bins captured crash concentrations 1.76x higher than baseline**, showing strong value for prioritizing attention and intervention.

### 2) Crash Cost Severity — Tweedie XGBoost

| Metric | Result |
|--------|--------|
| RMSE | **~$600,000** |
| MAE | **~$240,000** |
| Cost range modeled | **~$20K to $4M+** |

➡️ Crash costs were highly skewed, with a small number of very expensive incidents driving a large share of total losses. While exact cost prediction remained challenging, the model still helped highlight **which conditions are associated with higher-severity crashes**.

### 3) Crash Count Estimation — Hurdle Model

➡️ The hurdle model separated:
- **whether a crash happens**
- **how many crashes happen if one occurs**

In practice, most location-hour bins had **zero crashes**, and most non-zero cases had only **one crash**, so the count model added limited operational value. The strongest business value came from the **occurrence model**, which better supported prioritization and planning.

---

## 🔍 Key Insights
- **Crash risk increases during busy daytime and evening hours**, especially around commuting patterns.  
- **Rainfall** is one of the strongest signals associated with increased crash likelihood.  
- **Higher-speed corridors** are linked not only to more crashes, but also to **more severe and costly crashes**.  
- Certain high-risk patterns repeatedly appeared around **major Austin corridors and central urban zones**.  
- The model showed that crash risk is influenced not only by location history, but also by **current time and environmental conditions**, making it useful for forward-looking planning.  

---

## 🚀 Impact
- Transformed raw crash data into a **decision-support tool** for identifying high-risk areas and times.  
- Helped shift the use case from **reactive reporting** to **proactive risk management**.  
- Showed that focusing on the **top 5% highest-risk location-time bins** can improve targeting efficiency by **1.76x versus baseline**.  
- Created a framework that can support better decisions for:
  - **City transportation teams**
  - **Public safety and EMS allocation**
  - **Insurance pricing and claims risk assessment**
  - **Rideshare and delivery route planning**

### Business Impact / Opportunity
- Austin crash-related public costs were estimated at roughly **$35.24M in 2022 dollars**, highlighting the scale of the problem.  
- Even small improvements in identifying risky corridors, conditions, or time windows could create value through:
  - fewer crash-related disruptions
  - smarter infrastructure investment
  - better emergency response readiness
  - reduced insurance and operational losses  

This project demonstrates how predictive analytics can support **resource prioritization, operational efficiency, and risk-based decision-making** in both public and private sector mobility contexts.

---

## 🔮 Future Work
- Add **real-time traffic and event data** to improve short-term risk prediction  
- Build an **interactive dashboard or live risk map** for decision-makers  
- Improve severity modeling with richer roadway and post-crash features  
- Explore **network-aware models** such as Graph Neural Networks for better road-system representation  

---

## 🛠️ Tech Stack
- **Languages**: Python, SQL  
- **Libraries**: pandas, numpy, scikit-learn, LightGBM, XGBoost, shap, h3, matplotlib  
- **Data Sources**: Austin Open Data, weather data  
- **Tools**: Jupyter Notebook, geospatial analysis workflows  

---
