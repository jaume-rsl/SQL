# Table of contents:  
1. [JOINS](#joins)
2. [Simple queries](#Simple-queries)  
    * [Sorted queries](#Sorted-queries)    
    * [Filtering queries](#Filtered-queries)    
3. [Variables](#variables)
4. [Temporary Tables](#temporary-tables)
5. [Altering databases](#altering-databases)

# SQL&nbsp;&nbsp;&nbsp; ![under_construction](https://github.com/jaume-rsl/jaume-rsl/blob/d2fe9e9e4d973e7dbbc99aa49dacb8dc324e8039/images/under_construction.png)

## JOINS
### INNER
INNER with 3 tables
```SQL
SELECT track_id,
-- Enter the correct table name prefix when retrieving the name column from the track table
  track.name AS track_name,
  title as album_title,
  -- Enter the correct table name prefix when retrieving the name column from the artist table
  artist.name AS artist_name
FROM track
  -- Complete the matching columns to join album with track, and artist with album
INNER JOIN album on track.album_id = album.album_id 
INNER JOIN artist on album.artist_id = artist.artist_id;
```
![INNER](https://github.com/jaume-rsl/SQL/blob/4888248ac4a8d66c015e5725a81152960e20c79c/Screenshots/07%20-INNER%20JOIN.jpg)


## Simple queries
From an **Eurovision festival dataset**:
```SQL
-- Return unique countries and use an alias
  SELECT 
    DISTINCT country AS unique_country 
  FROM 
    eurovision;
```
![Alias](https://github.com/jaume-rsl/SQL/blob/a5f2b1378de02e5ced6b39a50f4ff5bb2b519843/Screenshots/01%20-%20Alias.jpg)


```SQL
-- Return all columns, restricting the N percent of rows returned
SELECT 
  TOP (50) PERCENT *
FROM 
  eurovision;
```
![TOP N %](https://github.com/jaume-rsl/SQL/blob/a5f2b1378de02e5ced6b39a50f4ff5bb2b519843/Screenshots/02%20-%20Top%20N%20percent.jpg)

### Sorted queries
From the US **power grid outages** dataset:
```SQL
-- Select the top 20 rows from description, nerc_region and event_date
SELECT 
  TOP (20) description,
  nerc_region,
  event_date
FROM 
  grid 
  -- Order by nerc_region, affected_customers & event_date
  -- Event_date should be in descending order
ORDER BY
  nerc_region,
  affected_customers,
  event_date DESC;
```
![TOP N sorted](https://github.com/jaume-rsl/SQL/blob/a5f2b1378de02e5ced6b39a50f4ff5bb2b519843/Screenshots/03%20-%20Top%20N%20sorted.jpg)

### Filtered queries
```SQL
-- Select description, affected_customers and event date
SELECT 
  description, 
  affected_customers,
  event_date
FROM 
  grid 
  -- The affected_customers column should be >= 50000 and <=150000   
WHERE 
  affected_customers BETWEEN 50000
  AND 150000 
   -- Define the order   
ORDER BY 
  event_date DESC;
```
![WHERE and ORDER BY](https://github.com/jaume-rsl/SQL/blob/57137a241952492ca81b3b93056e4cff5fb80d26/Screenshots/04%20-%20WHERE%20BETWEEN%20and%20SORTED.jpg)

With LIKE, BOOL and parenthesis:
```SQL
SELECT 
  artist, 
  release_year, 
  song 
FROM 
  songlist 
  -- Choose the correct artist and specify the release year
WHERE 
  (
    artist LIKE 'B%' 
    AND release_year = 1986
  ) 
  -- Or return all songs released after 1990
  OR release_year > 1990 
  -- Order the results
ORDER BY 
  release_year, 
  artist, 
  song;
```
![LIKE BOOL and parenthesis](https://github.com/jaume-rsl/SQL/blob/3b7047004fc2c9a2219f4ba34f651bb9d791fa76/Screenshots/05%20-%20LIKE,%20BOOL%20and%20parenthesis%20combination.jpg)


Filtering AGGREGATE functions with HAVING
```SQL
SELECT 
  country, 
  COUNT (country) AS country_count, 
  AVG (place) AS avg_place, 
  AVG (points) AS avg_points, 
  MIN (points) AS min_points, 
  MAX (points) AS max_points 
FROM 
  eurovision 
GROUP BY 
  country 
  -- The country column should only contain those with a count greater than 5 (aggregate func. with HAVING)
HAVING 
  COUNT(country) > 5 
  -- Arrange columns in the correct order
ORDER BY 
  avg_place, 
  avg_points DESC;
```
![HAVING](https://github.com/jaume-rsl/SQL/blob/c9cb6b3204c544b6542bee40a1eb7c025dcf0570/Screenshots/06%20-%20HAVING.jpg)

## Variables
```SQL
-- Declare your variables
DECLARE @start DATE
DECLARE @stop DATE
DECLARE @affected INT;
-- SET the relevant values for each variable
SET @start = '2014-01-24'
SET @stop  = '2014-07-02'
SET @affected =  5000 ;

SELECT 
  description,
  nerc_region,
  demand_loss_mw,
  affected_customers
FROM 
  grid
-- Specify the date range of the event_date and the value for @affected
WHERE event_date BETWEEN @start AND @stop
AND affected_customers >= @affected;
```
![Variables](https://github.com/jaume-rsl/SQL/blob/97134aa2875cb661f3c9f631a6cffa016189a880/Screenshots/08%20-%20Variables.jpg)

## Temporary Tables
```SQL
SELECT  album.title AS album_title,
  artist.name as artist,
  MAX(track.milliseconds / (1000 * 60) % 60 ) AS max_track_length_mins
-- Name the temp table #maxtracks
INTO #maxtracks
FROM album
-- Join album to artist using artist_id
INNER JOIN artist ON album.artist_id = artist.artist_id
-- Join track to album using album_id
INNER JOIN track ON track.album_id = album.album_id
GROUP BY artist.artist_id, album.title, artist.name,album.album_id
-- Run the final SELECT query to retrieve the results from the temporary table
SELECT album_title, artist, max_track_length_mins
FROM  #maxtracks
ORDER BY max_track_length_mins DESC, artist;
```
![Temporary Tables](https://github.com/jaume-rsl/SQL/blob/74e2182f350704573da859b2a52a4858298086f0/Screenshots/09%20-%20Temp%20Table.jpg)

## Altering databases

### Migrating data from a dataset to a dimension table
```SQL
-- Insert unique affiliations into affiliations blank table
INSERT INTO affiliations 
SELECT DISTINCT firstname, lastname, function, organization 
FROM university_professors;
```

### Changing datatypes (CAST)
```SQL
SELECT transaction_date, amount + CAST(fee AS integer) AS net_amount -- fee was a string in the beginning
FROM transactions;
```
![CAST](https://github.com/jaume-rsl/SQL/blob/41778522040d906ef2800b9bd0a7fbfedab1eae1/Screenshots/10%20-%20CAST.jpg)

### Truncate 'str' to x CHAR before reduce length
```SQL
ALTER TABLE professors 
ALTER COLUMN firstname 
TYPE varchar(16)
USING SUBSTRING(firstname FROM 1 FOR 16); -- firstname truncated selecting char n.1 to char n.16
```
