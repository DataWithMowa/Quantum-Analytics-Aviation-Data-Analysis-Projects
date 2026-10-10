# Airplane Crashes & Fatalities Dashboard (1908–2009)

<img width="947" height="467" alt="Airplane_Crashes   Fatalities Project" src="https://github.com/user-attachments/assets/7d57ee94-92af-4137-9fda-e8fac6d6e67d" />

### 📊 Project Overview

This project was built to explore **over 100 years of airplane crashes, fatalities, and aviation history**, from 1908 to 2009, and understand how aviation safety and accident patterns changed over time.

Using Power BI, I cleaned and analyzed historical airplane crash data and built an interactive dashboard to explore **accident trends, fatalities, survival rates, locations, operators, and aircraft types**.

The analysis also explored:

* How airplane crashes and fatalities changed over the years.
* The relationship between the number of people aboard and the number of fatalities.
* Which aircraft operators and types appeared most frequently in the accident records.
* How survival and fatality rates compared across the dataset.
* What historical accident records can teach us about risk and aviation safety.

This project became more than a data analysis exercise for me. It was an opportunity to explore how an industry has evolved through technology, investigations, experience, and lessons learned from past accidents.

### 🕹️ Interactive Dashboard Demo

See the dashboard filters, dynamic metrics, and charts in action below (66-second walkthrough): 

https://github.com/user-attachments/assets/f90775cf-ef56-41d6-9a07-efeef908d519

### To see the full live analysis: [Click here](https://lnkd.in/p/eqDJAHWN)

### 💡 Key Data Insights & Discoveries

1. **Aviation Has a Long History of Risk and Learning:** The dataset covers more than 100 years of aviation history, showing how much experience the industry has accumulated since its early years.

2. **Fatalities Accounted for a Significant Share of Recorded Outcomes:** Across the dataset, approximately 73% of the recorded accident outcomes were fatal, while 27% were survivals. These figures describe this dataset and should not be interpreted as the probability of dying on any flight.

3. **Aviation Has Evolved Significantly Over Time:** Aircraft technology, regulations, and safety practices have developed over the decades. Historical crash records provide important context for studying this evolution, although the dataset alone cannot prove what caused the improvements.

4. **Every Accident Provides an Opportunity to Learn:** Accident summaries contain details about what happened, including possible technical failures, operational challenges, and other contributing factors. These records show why investigating accidents is important.

5. **Historical Data Helps Us Ask Better Safety Questions:** Studying crashes across different periods, aircraft types, and operators helps us identify patterns and ask more informed questions about aviation risk and safety.

### 🛠️ Power BI Skills & Dashboard Setup

To turn over a century of plane crash records into one clear story, I used Power BI's data cleaning, data modeling, DAX, and dashboard features:

* **Data Cleaning & Preparation (Power Query):** Loaded the `Airplane` table from the Excel file, with 5,268 accidents from 1908 to 2009.
  * Set the right data types, such as `Date` as date, `Time` as time, and `Aboard`, `Fatalities` and `Ground_Fatalities` as whole numbers
  * **Split the `Route` column** at the dash into two new columns, `Origin` and `Destination`, so every crash has a start and an end city
  * Added a **Year** column from the date, then a **Decade** column (e.g. 1950, 1960) so crashes can be grouped by decade
  * Replaced blank and empty `Operator` values with "Unknown"
  * Added an **Operator Category** column that sorts each crash into **Military**, **Commercial/Civilian**, or **Unknown**
  
  <img width="155" height="113" alt="image" src="https://github.com/user-attachments/assets/db408b24-77cf-4af0-a42e-57c6b3f30376" />

  * Built a separate **Flight Routes** table by unpivoting `Origin` and `Destination` into one `City` column, with a `Path_ID`, `Point Order` and `Point Label` for each stop, so crash routes can be drawn on a map.
 
<img width="929" height="401" alt="image" src="https://github.com/user-attachments/assets/7203d89c-a1d2-4b40-85bf-0eb1879c8b89" />

* **Data Modeling:** Kept the model simple. A `Calendar` date table (built from the first to the last accident date) connects to the `Airplane` table with a one-to-many relationship on `Date`, so every chart can be sliced by date. The `Flight Routes` table is used for the route map visual.

<img width="653" height="343" alt="image" src="https://github.com/user-attachments/assets/0af78223-5597-45a3-8acd-1bb7f9d9f7da" />

* **DAX Measures:** Kept all measures in one dedicated `Measures (2)` table:

| Measure | What it does | DAX |
|---|---|---|
| **Total Accidents** | Counts every recorded crash | `COUNTROWS(Airplane)` |
| **Total Persons on Board** | Adds up everyone on the planes | `SUM(Airplane[Aboard])` |
| **Total Fatalities** | Adds up all deaths on board | `SUM(Airplane[Fatalities])` |
| **Total Ground Fatalities** | Adds up deaths on the ground | `SUM(Airplane[Ground_Fatalities])` |
| **Total Survivors** | People on board minus deaths | `[Total Persons on Board] - [Total Fatalities]` |
| **Fatality Rate** | Share of people on board who died | `DIVIDE([Total Fatalities], [Total Persons on Board], 0)` |
| **Survival Rate** | Share of people on board who lived | `DIVIDE([Total Survivors], [Total Persons on Board], 0)` |

<img width="211" height="153" alt="image" src="https://github.com/user-attachments/assets/34e60a5f-7f6e-40a0-b731-155b8f2474c9" />

* **Key Metric Tracking:** Created KPI cards for the main numbers: **Total Accidents (5,268), Total Persons on Board (144,551), Total Fatalities (105,479), Total Survivors (39,072), Fatality Rate (73%) and Survival Rate (27%).**

<img width="316" height="67" alt="image" src="https://github.com/user-attachments/assets/da6d71fd-df41-4a7b-945a-cadbe0bb29ea" />

* **Chart Analysis:** Built visuals to show survival rate by decade, crashes by operator, crashes by operator category, and where the crashes happened on a map.

*It's all in the dashboard image*

* **Interactive Slicers:** Added slicers so users can filter the whole dashboard by time period and other details.

<img width="353" height="62" alt="image" src="https://github.com/user-attachments/assets/c2dfe8f7-2afe-4acc-b215-521ccc95db07" />

### 📈 Strategic Recommendations & Lessons Learned

* **Continue investing in aviation safety:** Technology, aircraft design, maintenance, and safety procedures should continue to improve as new risks and lessons emerge.

* **Learn from every accident:** Accident investigations should identify contributing factors and help prevent similar incidents from happening again.

* **Use historical data to identify patterns:** Aviation authorities, researchers, and industry professionals can study accident records to guide further investigations and safety decisions.

* **Improve the quality of historical records:** More complete information on accident causes, flight hours, aircraft models, and operational conditions would support deeper analysis and more reliable comparisons.

* **Include newer data for current insights:** This dataset ends in 2009. Adding more recent records would allow the analysis to explore aviation accident patterns beyond the period covered by this project.

### 📂 How to Open and Explore the Dashboard

1. Download the Power BI file: [`Airplane_Crashes_and_Fatalities_Project.pbix`](https://github.com/DataWithMowa/Quantum-Analytics-Aviation-Data-Analysis-Projects/tree/main/Airplane%20Crashes%20and%20Fatalities%20Power%20BI%20Project/Full%20Project).
2. Open the file using **Power BI Desktop**.
3. Navigate through the dashboard pages and visuals.
4. Use the available filters and slicers to explore accident records and compare patterns across different categories and periods.

### 📁 Dataset Information

* **Dataset:** Airplane Crashes & Fatalities
* **Period Covered:** 1908–2009
* **Number of Records:** 5,268
* **File Format:** CSV
* **Tools Used:** Power BI, Power Query, DAX

**Important note:** This is a historical accident dataset, not a complete record of all flights. Its accident and fatality figures should not be used on their own to calculate the risk of flying or to prove changes in aviation safety.

### 🤝 Connect & Support

Thank you for taking the time to explore this project! If you have any questions or feedback, feel free to connect with me.

* 💼 **LinkedIn:** [Mowaninuola Umarudeen](https://www.linkedin.com/in/mowaninuolaumarudeen/)
* 📧 **Email:** [mowatheanalyst@gmail.com](mailto:mowatheanalyst@gmail.com)

📊 **Did you find this project useful?** Consider giving this repository a ⭐ Star!
