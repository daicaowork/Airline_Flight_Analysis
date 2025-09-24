
# ✈️ Airline Flight Pricing Analysis

## 📝 Project Introduction  

This project explores airline flight pricing patterns using SQL queries on a flight dataset (Delhi → Mumbai case study).  
The main goal is to uncover insights about pricing distribution, airline strategies, flight durations, and stopover effects.  

**Dataset:** Includes airline, flight number, source city, destination city, departure/arrival times, stops, class, duration, days left until departure, and ticket price.  
**Tools Used:** SQL Server (queries and analysis).  
**SQL Techniques Applied:** Aggregate Functions, Conditional Logic, Window Functions, CTEs, Subqueries, and Data Grouping.  

---

## 📂 Business Questions & SQL Queries  

### 1. Most Expensive Flight  
```sql
SELECT *
FROM airlines_data
WHERE price = (SELECT MAX(price) FROM airlines_data);
```
### 2.Cheapest Flight
```sql
SELECT *
FROM airlines_data
WHERE price = (SELECT MIN(price) FROM airlines_data);
```
### 3.Top 5 Most Expensive Flights
```sql
SELECT TOP 5 *
FROM airlines_data
ORDER BY price DESC;
```
### 4. Average Ticket Price by Airline
```sql
SELECT 
    airline,
    COUNT(*) AS num_flights,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY airline;
```
### 5. Min, Max, Avg Price per Airline
```sql
SELECT 
    airline,
    MIN(price) AS min_price,
    MAX(price) AS max_price,
    AVG(price) AS avg_price,
    COUNT(*) AS num_flights
FROM airlines_data
GROUP BY airline;
```
### 6. Percentage of Direct Flights (Zero Stop) by Airline
```sql
SELECT
    airline,
    ROUND(100.0 * SUM(is_direct) / COUNT(*), 2) AS pct_direct
FROM (
    SELECT *,
           CASE WHEN stops = 'zero' THEN 1 ELSE 0 END AS is_direct
    FROM airlines_data
) fd
GROUP BY airline;
```
### 7. Average Price by Departure Time
```sql
SELECT
    departure_time,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY departure_time
ORDER BY departure_time;
```
### 8. Average Price by Arrival Time
```sql
SELECT
    arrival_time,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY arrival_time
ORDER BY arrival_time;
```
### 9. Average Price by Duration Group
```sql
SELECT
    duration_group,
    AVG(price) AS avg_price
FROM (
    SELECT *,
           CASE
               WHEN duration < 3 THEN '<3h'
               WHEN duration BETWEEN 3 AND 6 THEN '3-6h'
               WHEN duration BETWEEN 6 AND 12 THEN '6-12h'
               WHEN duration BETWEEN 12 AND 20 THEN '12-20h'
               ELSE '>20h'
           END AS duration_group
    FROM airlines_data
) fd
GROUP BY duration_group
ORDER BY duration_group;
```
### 10. Average Price by Days Left
```sql
SELECT
    days_left,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY days_left
ORDER BY days_left;
```
### 11. Average Price by Airline (Only 1 Day Left)
```sql
SELECT 
    airline,
    AVG(price) AS avg_price
FROM airlines_data
WHERE days_left = 1
GROUP BY airline
ORDER BY avg_price;
```
### 12. Avg Price by Airline × Stops
```sql
SELECT 
    airline,
    stops,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY airline, stops
ORDER BY airline, stops;
```
### 13. Airlines with One-Stop Flights Cheaper than Direct Flights
```sql
WITH one_stop AS (
    SELECT airline, AVG(price) AS avg_price_one
    FROM airlines_data
    WHERE stops = 'one'
    GROUP BY airline
),
zero_stop AS (
    SELECT airline, AVG(price) AS avg_price_zero
    FROM airlines_data
    WHERE stops = 'zero'
    GROUP BY airline
)
SELECT
    o.airline,
    o.avg_price_one,
    z.avg_price_zero
FROM one_stop o
JOIN zero_stop z
    ON o.airline = z.airline
WHERE o.avg_price_one < z.avg_price_zero;
```
