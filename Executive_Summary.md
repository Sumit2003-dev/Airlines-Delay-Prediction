# Executive Summary: Flight Delay Analysis & Operational Insights

**Project by:** Sumit Kumar  
**Context:** Hands-on Data Analytics Project (IBM Data Science Learning Path)  
**Dataset:** 124,611 Cleaned Commercial Flight Records  

---

## 1. Why This Project Matters
Flight delays aren't just inconvenient for passengers—they cost airlines millions in crew overtime, gate repositioning, and fuel burn. When one flight is late early in the day, the same aircraft carries that delay into every subsequent trip on its schedule.

The goal of this project was twofold:
1. **Find out what's really driving flight delays** by digging into departure times, flight durations, airline carriers, and airport hubs.
2. **Test whether standard machine learning models can accurately predict delays** before departure, and understand the limits of what the data can tell us.

---

## 2. What the Numbers Show

* **Overall Network Baseline:** Out of 124,611 clean flights, **65.65% (81,803 flights) departed on time** and **34.35% (42,808 flights) were delayed**. This means any model has to beat a 65.65% benchmark (simply guessing "on time" every time).
* **Delays Compound Throughout the Day:** Morning flights (6:00 AM – 12:00 PM) have the highest reliability. As the day progresses, turnaround buffers get eaten up, causing delay rates to peak in the late afternoon and evening (4:00 PM – 9:00 PM).
* **Carrier Concentration:** Four airlines operate nearly half of all flights in this dataset:
  * Southwest Airlines (`WN`): **22,904 flights**
  * Delta Air Lines (`DL`): **14,931 flights**
  * SkyWest Airlines (`OO`): **12,090 flights**
  * American Airlines (`AA`): **11,129 flights**
* **Hub Bottlenecks:** The busiest departure points—Atlanta (`ATL`: 7,023 flights), Chicago O'Hare (`ORD`: 6,047 flights), and Dallas/Fort Worth (`DFW`: 5,349 flights)—act as central choke points. A delay at these hubs ripples across the entire network.

---

## 3. Modeling Results & Lessons Learned

| Model | Accuracy | Recall on Delays | What the Model Taught Us |
| :--- | :---: | :---: | :--- |
| **Logistic Regression** | **69.63%** | **33%** | Decent overall accuracy, but catches only 1 out of 3 actual delays due to class imbalance. |
| **Decision Tree** | **63.99%** | **47%** | Caught more delays, but overfit the training data and scored below the 65.65% baseline on test data. |
| **Random Forest** | **ROC-AUC: 0.70** | — | Showed the best balance in probability ranking across the full dataset. |

### Honest Technical Reflection:
In the Random Forest feature importance, `Flight` number showed up as the second most important feature (~10.3%). In reality, flight number is just an ID/code that was treated as a continuous number by the tree. The true operational drivers are **departure time** (time of day) and **flight length**. In a production setup, route IDs should be grouped by route corridor or target-encoded rather than fed in as raw numbers.

---

## 4. Practical Takeaways for Airline Operations

1. **Add Schedule Buffers for Afternoon Flights at Hubs:** Instead of keeping uniform 35-minute turn times all day, adding a 15-minute buffer for departures after 2:00 PM at congested hubs (`ORD`, `ATL`, `DFW`) would absorb delays before they cascade into evening flights.
2. **Dynamic Ground Crew Scheduling:** Shift standby maintenance and baggage handlers to afternoon and evening peaks at top 4 hubs, especially on Thursdays and Fridays when flight volumes are highest.
3. **Adjust Alert Thresholds for High-Value Routes:** Rather than using a strict 50% probability cutoff, lower the alert threshold on long-haul flights so operations teams get early warnings to swap gates or notify connecting passengers in advance.
