# Data Dictionary, KPIs & Spreadsheet Analysis Guide

This guide breaks down the core metrics, dataset definitions, data-cleaning decisions, and how to analyze this flight data using standard spreadsheet tools like Excel or Google Sheets.

---

## 1. Core KPIs & Formulas

### On-Time Departure Rate (OTDR)
* **What it measures:** The percentage of flights that departed without a delay flag.
* **Formula:**  
  `OTDR = (Count of On-Time Flights / Total Scheduled Flights) * 100`
* **In this dataset:** **65.65%** (81,803 on-time flights out of 124,611 total).

### Delay Rate
* **What it measures:** The percentage of flights that experienced a delay.
* **Formula:**  
  `Delay Rate = (Count of Delayed Flights / Total Scheduled Flights) * 100`
* **In this dataset:** **34.35%** (42,808 delayed flights).

### Top-Carrier Traffic Share
* **What it measures:** What proportion of the dataset is handled by the four busiest carriers (`WN`, `DL`, `OO`, `AA`).
* **Formula:**  
  `Top 4 Share = (Sum of Top 4 Carrier Flights / Total Flights) * 100`
* **In this dataset:** **49.0%** (61,054 flights).

---

## 2. Data Dictionary & Cleaning Notes

| Field Name | Type | Example | How it was cleaned / handled |
| :--- | :--- | :--- | :--- |
| `id` | Integer | `1`, `245` | Unique record ID. Dropped before training models so it wouldn't act as a fake predictor. |
| `Airline` | Text (2 letters) | `WN`, `DL`, `AA` | Clean carrier codes across 18 unique airlines. Handled with One-Hot Encoding. |
| `Flight` | Number | `269`, `1558` | Flight route number. |
| `AirportFrom` | Text (3 letters) | `SFO`, `ATL`, `ORD` | Origin airport IATA code. Non-null categorical field. |
| `AirportTo` | Text (3 letters) | `IAH`, `DFW`, `SEA` | Destination airport IATA code. Non-null categorical field. |
| `DayOfWeek` | Integer | `1` to `7` | Day of travel (1 = Monday, 7 = Sunday). |
| `Time` | Integer | `15`, `550`, `1015` | Scheduled departure time in minutes from midnight (e.g., 550 min = 09:10 AM). |
| `Length` | Number | `130`, `205` | Flight duration in minutes. The single missing value was imputed with the column mean (~130.66 mins). |
| `Delay` | Binary | `0` or `1` | Target variable (0 = On-Time, 1 = Delayed). The single record with missing delay was removed. |

---

## 3. How to Analyze This Data in Excel / Google Sheets

If you want to recreate these insights in Excel or Google Sheets, here are the exact formulas and Pivot Table setups:

### Useful Excel Formulas

* **Calculate the delay rate for a specific airline (e.g., Southwest / WN):**
  ```excel
  =COUNTIFS(B:B, "WN", I:I, 1) / COUNTIF(B:B, "WN")
  ```

* **Convert the minute timestamp into an hour number (0 to 23):**
  ```excel
  =INT(G2 / 60)
  ```

* **Group hours into Time-of-Day buckets (Morning, Afternoon, Evening, Late Night):**
  ```excel
  =IFS(INT(G2/60) < 6, "Late Night", INT(G2/60) < 12, "Morning", INT(G2/60) < 18, "Afternoon", TRUE, "Evening")
  ```

* **Look up airport city names from a lookup sheet:**
  ```excel
  =XLOOKUP(D2, AirportReference!A:A, AirportReference!B:B, "Unknown")
  ```

---

### Recommended Pivot Table Setups

1. **Carrier Delay Comparison:**
   * **Rows:** `Airline`
   * **Values:** `id` (Summarized by Count) and `Delay` (Summarized by Average, formatted as Percentage).
   * **Sort:** Average Delay descending to see the highest-risk carriers first.

2. **Hourly Delay Heatmap:**
   * **Rows:** `Departure_Hour` (0 through 23)
   * **Columns:** `DayOfWeek` (1 through 7)
   * **Values:** `Delay` (Summarized by Average, formatted as Percentage).
   * **Visual:** Apply conditional formatting (Green to Red color scale) to quickly spot peak congestion windows.
