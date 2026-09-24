# Mobility-Booking-App-Trips-Analysis


A SQL-driven analytics project on ride-booking trip data, culminating in an interactive Power BI dashboard covering conversion funnels, driver performance, location trends, and cancellation patterns.

<img width="1422" height="813" alt="image" src="https://github.com/user-attachments/assets/2205dd6d-5150-4bdf-bb83-e01994c07eca" />


## 📊 Project Overview

This project analyzes trip data from a mobility booking platform (Namma Yatri) to answer key operational and business questions, including:

- How efficient is the search → estimate → quote → booking funnel?
- Which drivers, locations, and durations perform best?
- Where are cancellations concentrated, and by whom?
- What payment methods and fare patterns stand out?

All analysis is done in SQL (window functions, subqueries, ranking) and visualized in a Power BI dashboard.

## 🗂️ Data Model

The analysis is built on five core tables:

| Table          | Description                                            |
|----------------|---------------------------------------------------------|
| `trips`        | Core trip records — driver, customer, fare, distance, duration, pickup location |
| `trip_details` | Funnel-level metrics per trip — searches, estimates, quotes, OTP entry, ride completion, cancellations |
| `assembly`     | Lookup table mapping location IDs to assembly/area names |
| `duration`     | Lookup table mapping duration IDs to duration labels |
| `payment`      | Lookup table mapping payment method IDs to method names |

## Meaning of each column in TRIPS Table

| Column                   | Meaning                                                                        | Example                         |
| ------------------------ | ------------------------------------------------------------------------------ | ------------------------------- |
| `tripid`                 | Unique ID for each trip/request                                                | `1` = Trip 1                    |
| `loc_from`               | Location from which the trip starts                                            | `16` = location ID 16           |
| `searches`               | Number of times the customer searched for a trip/ride                          | `1` = customer made a search    |
| `searches_got_estimate`  | Search resulted in an estimated fare/price being shown                         | `1` = estimate was provided     |
| `searches_for_quotes`    | Customer proceeded to request/look for actual quotes after seeing the estimate | `1` = customer requested quotes |
| `searches_got_quotes`    | Customer actually received one or more ride quotes                             | `1` = quote was received        |
| `customer_not_cancelled` | Customer did not cancel the trip                                               | `1` = customer continued        |
| `driver_not_cancelled`   | Driver did not cancel the trip                                                 | `1` = driver continued          |
| `otp_entered`            | OTP was entered to start/confirm the ride                                      | `1` = OTP entered               |
| `end_ride`               | Ride was successfully completed/ended                                          | `1` = ride ended                |


## 🛠️ Tech Stack

| Layer         | Tools                        |
|----------------|-------------------------------|
| Database       | SQL Server / T-SQL           |
| Analysis       | SQL (CTEs, window functions, subqueries, ranking) |
| Visualization  | Power BI                     |

## 📈 SQL Analysis

All queries live in [`trips_analysis.sql`](trips_analysis.sql). The analysis answers 26+ business questions, including:

**Volume & Funnel Metrics**
- Total completed trips, total drivers, total earnings, total distance traveled
- Full funnel breakdown: searches → got estimate → for quotes → got quotes → OTP entered → ride ended
- Conversion rates at each funnel stage (search-to-estimate, estimate-to-quote, quote acceptance, quote-to-booking)

**Driver & Payment Insights**
- Top 5 earning drivers (via `DENSE_RANK()` / `RANK()` window functions)
- Most-used and highest-value payment methods
- Driver–customer pairs with the most repeat trips

**Location & Duration Trends**
- Top locations by trip volume and by total fares
- Best-performing duration bucket by trip count and fare
- Highest-trip duration for each location, and highest-trip location for each duration (via `PARTITION BY`)

**Cancellations**
- Locations with the highest driver-initiated cancellations
- Locations with the highest customer-initiated cancellations

**Query techniques demonstrated:**
- Ranking with `RANK()` and `DENSE_RANK()`
- `PARTITION BY` for per-group top-N analysis
- Nested subqueries for multi-step aggregation
- Table lookups via `LEFT JOIN` to lookup tables (`assembly`, `duration`, `payment`)

## 📊 Power BI Dashboard

The dashboard (`Trips_Analysis_Dashboard.pbix`) includes:

- **KPI cards:** Completed Trips (983), Searches (2,161), Estimates (1,758), Quotes (1,277), Driver Earnings (751K)
- **Conversion Rate gauge:** Overall funnel conversion (0.45 on a 0–0.91 scale)
- **Trend charts:** Trips vs. Duration, Fare vs. Duration, Distance vs. Duration
- **Assembly-level table:** Searches, searches for quotes, and related funnel metrics broken down by assembly/area
- **Map visual:** Trip locations plotted geographically
- **Assembly filter:** Slicer to drill into a specific area or view all

## 🚀 How to Reproduce

1. **Clone the repo**
   ```bash
   git clone https://github.com/<your-username>/namma-yatri-trips-analysis.git
   cd namma-yatri-trips-analysis
   ```

2. **Set up the database**
   - Create the `trips`, `trip_details`, `assembly`, `duration`, and `payment` tables in your SQL Server (or compatible) instance
   - Load the source data into these tables

3. **Run the analysis**
   - Execute the queries in `trips_analysis.sql` to reproduce all metrics and rankings

4. **Open the dashboard**
   - Open `Trips_Analysis_Dashboard.pbix` in Power BI Desktop
   - Update the data source connection to point to your database
   - Refresh to load the latest data

## 📁 Project Structure

```
namma-yatri-trips-analysis/
├── trips_analysis.sql                 # All SQL analysis queries
├── Trips_Analysis_Dashboard.pbix      # Power BI dashboard
├── dashboard_preview.png              # Dashboard screenshot
└── README.md
```

## 🔍 Key Insights

- Of **2,161** total searches, only **983** trips were completed — highlighting drop-off across the funnel.
- Overall conversion sits at **~45%**, with room to improve between the estimate and quote stages.
- Trip volume, fares, and distance all show similar cyclical patterns across duration buckets.
- Assembly-level breakdowns reveal meaningful variation in search volume and quote conversion by area, useful for targeted driver allocation.


