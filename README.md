# ✈️ Commercial Airlines Delay Prediction & Analytics

A data analytics and machine learning case study exploring what factors contribute most to commercial flight delays across major U.S. carriers, using Python (`pandas`, `matplotlib`, `seaborn`, `scikit-learn`).

*Developed as a practical coursework project within the IBM Data Science learning path.*

---

## 📁 Repository Overview

| File | What's Inside |
| :--- | :--- |
| **[`Executive_Summary.md`](./Executive_Summary.md)** | A 1-page summary written for operations teams explaining key findings, bottleneck hubs, and practical scheduling recommendations. |
| **[`Analytics_Insights_and_KPIs.md`](./Analytics_Insights_and_KPIs.md)** | Full data dictionary, KPI formulas (OTDR, delay rates), and a step-by-step guide for analyzing this data in Excel / Google Sheets. |
| **[`IBM_PROJECT.ipynb`](./IBM_PROJECT.ipynb)** | The complete Jupyter Notebook with data cleaning, EDA charts, feature engineering, and model evaluations. |

---

## 📌 Project Goals
1. **Understand Operational Patterns:** Identify how departure time, flight length, day of week, carrier, and origin/destination airports correlate with delays.
2. **Build & Benchmark Predictive Models:** Train baseline classification algorithms (Logistic Regression, Decision Tree, Random Forest) to evaluate how well flight delays can be predicted before takeoff.
3. **Translate Findings into Actions:** Provide realistic operational suggestions for flight scheduling and ground crew resource allocation.

---

## 📊 Dataset Summary
* **Cleaned Records:** 124,611 commercial flights
* **Target Variable:** `Delay` (`0` = On-Time [65.65%], `1` = Delayed [34.35%])
* **Baseline Accuracy:** A naive model that predicts "on time" for every single flight scores **65.65%**. Any useful ML model needs to show meaningful predictive power above this number.

| Column | Type | Description |
| :--- | :--- | :--- |
| `id` | Integer | Flight identifier (dropped before modeling) |
| `Airline` | Categorical | Airline code (18 carriers including `WN`, `DL`, `OO`, `AA`) |
| `Flight` | Number | Flight route number |
| `AirportFrom` | Categorical | Departure airport code (`ATL`, `ORD`, `DFW`, `DEN`, etc.) |
| `AirportTo` | Categorical | Arrival airport code |
| `DayOfWeek` | Integer | Day of travel (1 = Monday, 7 = Sunday) |
| `Time` | Integer | Departure time in minutes from midnight (0–1439) |
| `Length` | Number | Flight duration in minutes |
| `Delay` | Binary | Target (0 = On-Time, 1 = Delayed) |

---

## 🔍 Key Data Insights

* **Delays Snowball Through the Day:** Early morning departures (6:00 AM – 12:00 PM) have the highest on-time rates. As aircraft complete consecutive legs, minor morning delays compound into major afternoon and evening delays (peaking between 4:00 PM and 9:00 PM).
* **Carrier Volumes:** Southwest Airlines (`WN`), Delta (`DL`), SkyWest (`OO`), and American Airlines (`AA`) represent nearly half of all flights in the dataset.
* **Hub Bottlenecks:** Atlanta (`ATL`), Chicago O'Hare (`ORD`), and Dallas/Fort Worth (`DFW`) handle the highest flight volumes and show the strongest delay ripple effects across connecting routes.
* **Peak Travel Days:** Mid-to-late week flights (Wednesday through Friday) experience noticeably higher traffic and delay variance than weekends.

---

## 📈 Model Performance & Key Learnings

| Model | Test Accuracy | Delay Precision | Delay Recall | ROC-AUC | Main Takeaway |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Logistic Regression** | **69.63%** | 0.61 | 0.33 | 0.69 | Modest +3.98% gain over baseline; struggles to catch delays (low recall) due to class imbalance. |
| **Decision Tree** | **63.99%** | 0.48 | 0.47 | 0.60 | Catches more delays, but overfits the training set and drops below baseline on test data. |
| **Random Forest** | *Evaluated* | — | — | **0.70** | Produced the strongest overall probability ranking and feature importance signals. |

### Technical Note on Feature Importance:
`Time` (scheduled departure) and `Length` (flight duration) were the primary real-world predictors. While `Flight` route number ranked high in the Random Forest feature importance (~10.3%), this is an artifact of treating route IDs as continuous numeric values. In a production pipeline, routes should be target-encoded or grouped by corridor.

---

## 💡 Practical Recommendations
* **Dynamic Turnaround Buffers:** Add 15-minute schedule buffers for departures after 2:00 PM at congested hub airports (`ORD`, `ATL`, `DFW`) to stop delays from cascading across an aircraft's daily rotation.
* **Targeted Crew Allocation:** Shift standby ground handlers and maintenance staff to peak afternoon banks on Thursdays and Fridays.
* **Proactive Passenger Alerts:** Use model probability scores to flag high-risk connecting flights 3–4 hours before departure, enabling earlier gate management.
