# US International Air Traffic Dashboard (1990–2020)

<img width="945" height="467" alt="Dashboard" src="https://github.com/user-attachments/assets/6c7a078d-c83f-4733-a8ce-3ee343edc848" />

### 📊 Project Overview

This project was built to explore **30 years of international air passenger traffic to and from the United States**, from 1990 to early 2020, and understand how travel grew, where people flew, and how the COVID-19 period first showed up in the numbers.

Using Power BI, I cleaned and analyzed US Department of Transportation (T-100) international passenger and departure records and built an interactive dashboard to explore **passenger volumes, airlines, US airports, foreign airports, world regions, and scheduled vs. charter flights**.

The analysis also explored:

* How international passenger numbers changed year by year from 1990 to 2019.
* Which world regions, airlines, and airports carried the most passengers.
* How scheduled flights compared with charter flights.
* How big events, such as 2001, 2009, and early 2020, showed up in the data.
* How the first months of COVID-19 affected passenger numbers.

This project helped me practice working with a large, messy dataset (over 680,000 passenger rows), fixing inconsistent names, and turning raw records into a clear story about how people move around the world.

### 🕹️ Interactive Dashboard Demo

See the dashboard filters, dynamic metrics, and charts in action below (60-second walkthrough):

https://github.com/user-attachments/assets/92820b59-380b-496f-931a-62f351b1c12b

### To see the full live analysis: [Click here](https://lnkd.in/p/ed326eqY)

### 💡 Key Data Insights & Discoveries

1. **International Travel Nearly Tripled in 30 Years:** Yearly passengers grew from about 84.4 million in 1990 to about 244.1 million in 2019. Across the full dataset, the total is about 4.55 billion passenger records.

2. **Europe Is the Biggest Region:** Europe carried about 1.44 billion passengers, roughly 32% of the total. Middle America (741 million), Far East / Asia (704 million), Canada and Greenland (617 million), and the Caribbean (493 million) follow.

3. **A Few Big Airports Carry a Lot of Traffic:** On the US side, JFK (635 million), LAX (492 million), and MIA (489 million) lead. On the foreign side, London Heathrow (328 million), Toronto (258 million), and Tokyo Narita (255 million) lead.

4. **A Few US Airlines Lead the List:** American Airlines (589 million), United (417 million), and Delta (384 million) are the top three carriers by passengers in this dataset.

5. **Scheduled Flights Make Up Almost Everything:** About 97.1% of passengers flew on scheduled flights, and only 2.9% flew on charter flights.

6. **Big Events Show Up in the Trend:** Passengers fell about 9.1% in 2001 and about 5.9% in 2009. The data ends in March 2020, and by then traffic was already falling sharply: March 2020 was about 56% lower than March 2019, and the first three months of 2020 were about 21% lower than the same months of 2019.

### 🛠️ Power BI Skills & Dashboard Setup

To turn two large government files into one clear picture of international air travel, I used Power BI's data cleaning, data modeling, DAX, and dashboard features:

* **Data Cleaning & Preparation (Power Query):** Loaded two CSV files from the US DOT T-100 program: `International_Report_Passengers` and `International_Report_Departures`.
  * Renamed column headers to a clean format with underscores and no spaces (e.g. `US_Airport_Name`, `Foreign_Airport_ID`, `Airline_Carrier_Name`)
  * Set the right data types, such as `Date` as date, and `Scheduled_Passengers`, `Charter_Passengers` and `Total_Passengers` as whole numbers
  * **Fixed inconsistent names:** the same airport or airline ID sometimes appeared with different name spellings over time (for example, one Istanbul airport ID showed up under two different codes). I grouped by ID, picked the **newest name** for each one, and applied it to US airports, foreign airports, and airlines
  * **Removed duplicate rows** that this problem created, leaving 680,985 clean passenger rows
  * Added a **Route** column (US airport + foreign airport) so each flight path can be analyzed on its own

<img width="708" height="26" alt="image" src="https://github.com/user-attachments/assets/c1ebee85-bb57-4a6d-96c2-0f31d0bb8f98" />

* **Data Modeling:** Built a small model around a date table and a region lookup table:
  * `Date_Table` is a calendar from January 1990 to December 2020, with `Year`, `Month Number`, and `Month Name` (sorted by month number)
  * `WAC_Region_Map` is a lookup table I created to turn World Area Codes into a **country** and a **world region** (Europe, Caribbean, Far East / Asia, and so on)
  * Both fact tables connect to `Date_Table` on `Date` and to `WAC_Region_Map` on the foreign airport region code, using one-to-many relationships, so charts can be sliced by time or by region

<img width="665" height="328" alt="image" src="https://github.com/user-attachments/assets/4fe01461-d0ce-4193-b13f-cdd37a117873" />

* **DAX Measures:** Kept all 13 measures in one dedicated `Measures (2)` table:

| Measure | What it does | DAX |
|---|---|---|
| **Total Passengers** | Adds up all passengers | `SUM(International_Report_Passengers[Total_Passengers])` |
| **Passengers PY** | Passengers in the same period last year | `CALCULATE([Total Passengers], SAMEPERIODLASTYEAR(Date_Table[Date]))` |
| **YoY Growth %** | Change compared with the same period last year | `DIVIDE([Total Passengers] - [Passengers PY], [Passengers PY])` |
| **Passengers 2019** | Passengers in 2019 only | `CALCULATE([Total Passengers], Date_Table[Year] = 2019)` |
| **Passengers 2020** | Passengers in 2020 only | `CALCULATE([Total Passengers], Date_Table[Year] = 2020)` |
| **COVID Drop %** | Change from 2019 to 2020 | `DIVIDE([Passengers 2020] - [Passengers 2019], [Passengers 2019])` |
| **Total Scheduled Passengers** | Passengers on scheduled flights | `SUM(International_Report_Passengers[Scheduled_Passengers])` |
| **Total Charter Passengers** | Passengers on charter flights | `SUM(International_Report_Passengers[Charter_Passengers])` |
| **Scheduled Share %** | Share of passengers on scheduled flights | `DIVIDE([Total Scheduled Passengers], [Total Passengers])` |
| **Charter Share %** | Share of passengers on charter flights | `DIVIDE([Total Charter Passengers], [Total Passengers])` |
| **Total Airlines** | Counts different airlines | `DISTINCTCOUNT(International_Report_Passengers[Airline_ID])` |
| **Total US Airports** | Counts different US airports | `DISTINCTCOUNT(International_Report_Departures[US_Airport_ID])` |
| **Total Foreign Airports** | Counts different foreign airports | `DISTINCTCOUNT(International_Report_Departures[Foreign_Airport_ID])` |

<img width="197" height="257" alt="image" src="https://github.com/user-attachments/assets/bfaad7ba-895b-45e7-afd3-9bd585e08a9b" />

* **Key Metric Tracking:** Created KPI cards for the main numbers: **Total Passengers (about 4.55 billion), Total Airlines (570), Total US Airports (1,015), Total Foreign Airports (1,666), Scheduled Share (97.1%) and Charter Share (2.9%).**

<img width="613" height="52" alt="image" src="https://github.com/user-attachments/assets/74d422f7-9b74-427e-8060-5efe97af0902" />

* **Chart Analysis:** Built visuals to track passengers over time, compare world regions, rank the top airlines, and rank the top US and foreign airports.

*It's all in the dashboard image*

* **Interactive Slicers:** Added slicers so users can filter the whole dashboard by year, region, and other details.

<img width="46" height="85" alt="image" src="https://github.com/user-attachments/assets/3d51aa49-c735-485e-9d7f-53b4d5720ec5" />

### 📈 Strategic Recommendations & Lessons Learned

* **Plan around the big routes and hubs:** A small group of airports, airlines, and regions carries a large share of passengers. Airlines, airports, and tourism boards can focus their planning on these.

* **Prepare for sudden drops:** The dips in 2001, 2009, and early 2020 show that international travel can change fast. Capacity and staffing plans should allow for this.

* **Keep names consistent in source data:** The same ID appearing under different names caused duplicate rows. A single, stable name for each airport and airline would make this kind of analysis faster and safer.

* **Compare like with like:** Because the data stops in March 2020, the fairest COVID comparison is the same months in 2019 and 2020, not full-year totals.

* **Add newer data:** Adding 2020 to the present would show the full COVID drop and how long recovery took.

### 📂 How to Open and Explore the Dashboard

1. Download the Power BI file: [`US_International_Air_Traffic_Project.pbix`](https://github.com/DataWithMowa/Quantum-Analytics-Aviation-Data-Analysis-Projects/tree/main/U.S.%20International%20Air%20Traffic%20Power%20BI%20Project/Full%20Project).
2. Open the file using **Power BI Desktop**.
3. Navigate through the dashboard pages and visuals.
4. Use the available filters and slicers to compare years, regions, airlines, and airports.

### 📁 Dataset Information

* **Dataset:** US DOT T-100 International Report (Passengers and Departures)
* **Period Covered:** January 1990 – March 2020
* **Number of Records:** 680,985 passenger rows (after cleaning) and 930,808 departure rows
* **File Format:** CSV
* **Tools Used:** Power BI, Power Query, DAX

**Important note:** The passenger figures count passengers on flight segments, so one person making a round trip is counted more than once. They should not be read as the number of different people who traveled. The data ends in March 2020, so 2020 is a partial year.

### 🤝 Connect & Support

Thank you for taking the time to explore this project! If you have any questions or feedback, feel free to connect with me.

* 💼 **LinkedIn:** [Mowaninuola Umarudeen](https://www.linkedin.com/in/mowaninuolaumarudeen/)
* 📧 **Email:** [mowatheanalyst@gmail.com](mailto:mowatheanalyst@gmail.com)

📊 **Did you find this project useful?** Consider giving this repository a ⭐ Star!
