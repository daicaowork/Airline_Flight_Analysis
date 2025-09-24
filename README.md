
# ✈️ Airline Flight Pricing Analysis using SQL  

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
