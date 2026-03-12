# Austin-Crash-Risk-Intelligence-System

This project explores how data science and machine learning can be used to **identify when and where traffic crashes are most likely to happen in Austin, and estimate their potential severity and cost**. Using Austin’s public crash dataset along with weather, temporal, and spatial features, I built predictive models that help turn historical crash records into **actionable risk insights for public safety, city planning, and mobility decision-making**.

---

## 📌 Project Overview
- **Goal**: Predict **crash likelihood** and analyze **crash severity/cost patterns** across Austin at the **location-hour level**.  
- **Why it matters**: Austin’s open crash data captures about **10 years** of incidents and roughly **224,000 raw records**, showing the scale of the traffic safety problem. With crash-related public costs estimated at **$35.24M in 2022**, a model that improves targeting of high-risk areas and times can support smarter operational and infrastructure decisions. 
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
- Periods/areas **where crashes occurred**
- Periods/areas **where no crashes occurred**

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
- **Whether a crash happens**
- **How many crashes happen if one occurs**

In practice, most location-hour bins had **zero crashes**, and most non-zero cases had only **one crash**, so the count model added limited operational value. The strongest business value came from the **occurrence model**, which better supported prioritization and planning.

---

## 🔍 Key Insights
- The dataset included ~**224,000 raw crash records**, which were cleaned into ~**220,000 validated crash records** and modeled across **47 features** spanning crash, time, location, roadway, and weather conditions.  
- Crash prediction was a **rare-event problem**, with crashes occurring in only about **0.2% of the full hexagon-hour grid**, making baseline prediction difficult and increasing the value of targeted risk ranking.  
- The best-performing model (**Binary LightGBM**) achieved **64.76% ROC AUC** and **7.35% PR AUC**, showing it could separate higher-risk from lower-risk conditions meaningfully despite extreme class imbalance.  
- The model’s **top 5% predicted high-risk bins captured crash concentrations 1.76x higher than baseline**, meaning the model can help decision-makers focus attention on a much smaller subset of locations and times with materially higher crash likelihood.  
- Crash risk increased during **daytime and evening travel periods**, and rainfall and higher-speed corridors consistently appeared among the strongest risk-related signals.  
- Severity modeling showed crash costs ranging from roughly **$20K to $4M+**, confirming that while most crashes are lower-cost, a small share of incidents drives disproportionately high financial impact.  

---

## 🚀 Impact
- Converted ~**220,000 cleaned crash records** into a **location-hour risk prediction framework** that supports proactive planning instead of relying only on historical reporting.  
- Demonstrated that focusing on the **top 5% highest-risk bins** improves crash targeting efficiency by **1.76x over baseline**, creating a more practical shortlist for intervention, monitoring, or operational deployment.  
- Built a framework that can help city and mobility stakeholders prioritize resources across thousands of location-time combinations rather than treating all road segments and hours equally.  
- Quantified the broader problem size by linking crash patterns to an estimated **$35.24M in public crash-related costs (2022 dollars)**, showing meaningful economic upside from even modest reductions in high-risk incidents.  
- Created business value across multiple use cases:
  - **Public sector**: better targeting of enforcement, signage, lighting, and corridor redesign investments  
  - **EMS / operations**: improved staging decisions during higher-risk times and conditions  
  - **Insurance**: more context-aware risk segmentation and pricing analysis  
  - **Rideshare / logistics**: safer routing and risk-informed driver or fleet planning  
- Even a small improvement in preventing crashes within the model’s highest-risk segments could translate into measurable gains through lower emergency response strain, fewer disruptions, and reduced public and private loss exposure.  

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
- **Tools**: Jupyter Notebook, Geospatial Analysis Workflows  

---
