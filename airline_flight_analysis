
/* =======================================================================
   1. Price Overview
   ======================================================================= */

-- 1.1 Most expensive flight
SELECT *
FROM airlines_data
WHERE price = (SELECT MAX(price) FROM airlines_data);

-- 1.2 Cheapest flight
SELECT *
FROM airlines_data
WHERE price = (SELECT MIN(price) FROM airlines_data);

-- 1.3 Top 5 most expensive flights
SELECT TOP 5 *
FROM airlines_data
ORDER BY price DESC;


/* =======================================================================
   2. Comparison by Airline
   ======================================================================= */

-- 2.1 Average ticket price per airline
SELECT 
    airline,
    COUNT(*) AS num_flights,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY airline;

-- 2.2 Min, max, and average price per airline
SELECT 
    airline,
    MIN(price) AS min_price,
    MAX(price) AS max_price,
    AVG(price) AS avg_price,
    COUNT(*) AS num_flights
FROM airlines_data
GROUP BY airline;

-- 2.3 Percentage of direct flights (zero stop) per airline
SELECT
    airline,
    ROUND(100.0 * SUM(is_direct) / COUNT(*), 2) AS pct_direct
FROM (
    SELECT *,
           CASE WHEN stops = 'zero' THEN 1 ELSE 0 END AS is_direct
    FROM airlines_data
) fd
GROUP BY airline;


/* =======================================================================
   3. Analysis by Flight Time
   ======================================================================= */

-- 3.1 Average ticket price by departure time
SELECT
    departure_time,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY departure_time
ORDER BY departure_time;

-- 3.2 Average ticket price by arrival time
SELECT
    arrival_time,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY arrival_time
ORDER BY arrival_time;


/* =======================================================================
   4. Analysis by Duration
   ======================================================================= */

-- 4.1 Group flights by duration and calculate average price
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


/* =======================================================================
   5. Analysis by Days Left
   ======================================================================= */

-- 5.1 Average ticket price by days left before departure
SELECT
    days_left,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY days_left
ORDER BY days_left;

-- 5.2 Average ticket price by airline (when only 1 day left)
SELECT 
    airline,
    AVG(price) AS avg_price
FROM airlines_data
WHERE days_left = 1
GROUP BY airline
ORDER BY avg_price;


/* =======================================================================
   6. Advanced Analysis (Multi-factor)
   ======================================================================= */

-- 6.1 Average price per airline × stop type
SELECT 
    airline,
    stops,
    AVG(price) AS avg_price
FROM airlines_data
GROUP BY airline, stops
ORDER BY airline, stops;

-- 6.2 Airlines with one-stop flights cheaper than direct flights
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
