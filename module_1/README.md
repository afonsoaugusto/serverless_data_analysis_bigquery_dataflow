# Module 1

## Serverless Data Analysis with BigQuery Lab 1: Building a BigQuery Query

```sql
-- Task 1. Create and run a query
SELECT
  airline,
  date,
  departure_delay
FROM
  `bigquery-samples.airline_ontime_data.flights`
WHERE
  departure_delay > 0
  AND departure_airport = 'LGA'
LIMIT
  100
```

```sql
-- Task 2. Aggregate and Boolean functions
SELECT
  airline,
  COUNT(departure_delay)
FROM
   `bigquery-samples.airline_ontime_data.flights`
WHERE
  departure_airport = 'LGA'
  AND date = '2008-05-13'
GROUP BY
  airline
ORDER BY airline

-- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
SELECT
  f.airline,
  COUNT(f.departure_delay) AS total_flights,
  SUM(IF(f.departure_delay > 0, 1, 0)) AS num_delayed
FROM
   `bigquery-samples.airline_ontime_data.flights` AS f
WHERE
  f.departure_airport = 'LGA' AND f.date = '2008-05-13'
GROUP BY
  f.airline
```

```sql
-- Task 3. String operations
SELECT
  CONCAT(CAST(year AS STRING), '-', LPAD(CAST(month AS STRING),2,'0'), '-', LPAD(CAST(day AS STRING),2,'0')) AS rainyday
FROM
  `bigquery-samples.weather_geo.gsod`
WHERE
  station_number = 725030
  AND total_precipitation > 0
```

```sql
-- Task 4. Join on Date
SELECT
  f.airline,
  SUM(IF(f.arrival_delay > 0, 1, 0)) AS num_delayed,
  COUNT(f.arrival_delay) AS total_flights
FROM
  `bigquery-samples.airline_ontime_data.flights` AS f
JOIN (
  SELECT
    CONCAT(CAST(year AS STRING), '-', LPAD(CAST(month AS STRING),2,'0'), '-', LPAD(CAST(day AS STRING),2,'0')) AS rainyday
  FROM
    `bigquery-samples.weather_geo.gsod`
  WHERE
    station_number = 725030
    AND total_precipitation > 0) AS w
ON
  w.rainyday = f.date
WHERE f.arrival_airport = 'LGA'
GROUP BY f.airline
```

```sql
-- Task 5. Subquery
SELECT
  airline,
  num_delayed,
  total_flights,
  num_delayed / total_flights AS frac_delayed
FROM (
SELECT
  f.airline AS airline,
  SUM(IF(f.arrival_delay > 0, 1, 0)) AS num_delayed,
  COUNT(f.arrival_delay) AS total_flights
FROM
  `bigquery-samples.airline_ontime_data.flights` AS f
JOIN (
  SELECT
    CONCAT(CAST(year AS STRING), '-', LPAD(CAST(month AS STRING),2,'0'), '-', LPAD(CAST(day AS STRING),2,'0')) AS rainyday
  FROM
    `bigquery-samples.weather_geo.gsod`
  WHERE
    station_number = 725030
    AND total_precipitation > 0) AS w
ON
  w.rainyday = f.date
WHERE f.arrival_airport = 'LGA'
GROUP BY f.airline
  )
ORDER BY
  frac_delayed ASC
```

## Serverless Data Analysis with BigQuery Lab 2: Loading and Exporting Data

```sh
curl https://storage.googleapis.com/cloud-training/CPB200/BQ/lab4/schema_flight_performance.json -o schema_flight_performance.json
```

```sh
bq load --source_format=NEWLINE_DELIMITED_JSON $DEVSHELL_PROJECT_ID:cpb101_flight_data.flights_2014 gs://cloud-training/CPB200/BQ/lab4/domestic_2014_flights_*.json ./schema_flight_performance.json
```

```sh
bq ls $DEVSHELL_PROJECT_ID:cpb101_flight_data
```

```sh
BUCKET="qwiklabs-gcp-1df9bd5c4cf4c142"
echo $BUCKET

gs://qwiklabs-gcp-1df9bd5c4cf4c142/bq/airports.csv
bq extract cpb101_flight_data.AIRPORTS gs://$BUCKET/bq/airports2.csv
```

## Serverless Data Analysis with BigQuery - Lab 3 : Advanced SQL Queries v1.3

```sql
SELECT
  author.email,
  diff.new_path AS path,
  DATETIME(TIMESTAMP_SECONDS(author.date.seconds)) AS date
FROM
  `bigquery-public-data.github_repos.commits`,
  UNNEST(difference) diff
WHERE
  EXTRACT(YEAR FROM TIMESTAMP_SECONDS(author.date.seconds))=2016
LIMIT 10

SELECT
  author.email,
  difference[OFFSET(0)].new_path AS path,
  DATETIME(TIMESTAMP_SECONDS(author.date.seconds)) AS date
FROM
  `bigquery-public-data.github_repos.commits`,
  UNNEST(difference) diff
WHERE
  EXTRACT(YEAR FROM TIMESTAMP_SECONDS(author.date.seconds))=2016
LIMIT 10
```

```sql
```

```sql
WITH commits AS (
  SELECT
    author.email,
    LOWER(REGEXP_EXTRACT(diff.new_path, r'\.([^\./\(~_ \- #]*)$')) lang,
    diff.new_path AS path,
    TIMESTAMP_SECONDS(author.date.seconds) AS author_timestamp
  FROM
    `bigquery-public-data.github_repos.commits`,
    UNNEST(difference) diff
  WHERE
    EXTRACT(YEAR FROM TIMESTAMP_SECONDS(author.date.seconds))=2016 )
SELECT
  lang,
  COUNT(path) AS numcommits
FROM
  commits
WHERE
  LENGTH(lang)<8
  AND lang IS NOT NULL
  AND REGEXP_CONTAINS(lang, '[a-zA-Z]')
GROUP BY
  lang
HAVING
  numcommits > 100
ORDER BY
  numcommits DESC
```

```sql
WITH commits AS (
  SELECT
    author.email,
    EXTRACT(DAYOFWEEK
    FROM
      TIMESTAMP_SECONDS(author.date.seconds)) BETWEEN 2
    AND 6 is_weekday,
    LOWER(REGEXP_EXTRACT(diff.new_path, r'\.([^\./\(~_ \- #]*)$')) lang,
    diff.new_path AS path,
    TIMESTAMP_SECONDS(author.date.seconds) AS author_timestamp
  FROM
    `bigquery-public-data.github_repos.commits`,
    UNNEST(difference) diff
  WHERE
    EXTRACT(YEAR
    FROM
      TIMESTAMP_SECONDS(author.date.seconds))=2016)
SELECT
  lang,
  is_weekday,
  COUNT(path) AS numcommits
FROM
  commits
WHERE
  lang IS NOT NULL
GROUP BY
  lang,
  is_weekday
HAVING
  numcommits > 100
ORDER BY
  numcommits DESC
```

```sql
```