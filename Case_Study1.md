# CASE STUDY 1 - Job Search Analysis
## 🤩 Queries
## 1. Number of jobs reviewed: Amount of jobs reviewed over time. task: Calculate the number of jobs reviewed per hour per day for November 2020?
```
SELECT COUNT( DISTINCT job_id)/(30*24) FROM job_data WHERE ds BETWEEN ‘2020 2020-11-01’ AND ‘2020 2020-11-30'
```

## 2. Throughput: It is the no. of events happening per second. task: Let’s say the above metric is called throughput. Calculate 7 day rolling average of throughput? For throughput, do you prefer daily metric or 7-day rolling and why?
```
SELECT ds, event_per_day,AVG(event_per_day)OVER(ORDER BY ds ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)AS 7_day_rolling_avg FROM
(SELECT ds, COUNT(DISTINCT event) AS event_per_day FROM job_data WHERE
ds BETWEEN ‘2020 2020-11 -01’ AND ‘2020 2020-11 -30’ GROUP BY ds ORDER BY ds)a
```

## 3. Percentage share of each language: Share of each language for different contents. task: Calculate the percentage share of each language in the last 30 days?
```
SELECT language, 100.0* num_jobs/total_jobs AS pct_share FROM
SELECT language, COUNT(DISTINCT job_id) AS num_jobs FROM job_data
GROUP BY Language)a
CROSS JOIN
SELECT COUNT(DISTINCT job_id) AS total_jobs FROM job_data)b ORDER BY pact_share DESC
```

## 4. Duplicate rows: Rows that have the same value present in them. task: Let’s say you see some duplicate rows in the data. How will you display duplicates from the table?
```
SELECT * FROM
( SELECT *, ROW_NUMBER()OVER(PARTITION BY job_id) AS rownum
FROM job_dat a )a WHERE rownum > 1
```
