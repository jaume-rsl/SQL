# SQL

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

