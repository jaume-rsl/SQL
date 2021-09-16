# SQL

```SQL
-- Return unique countries and use an alias
  SELECT 
    DISTINCT country AS unique_country 
  FROM 
    eurovision;
```
![Alias](https://github.com/jaume-rsl/SQL/blob/12858852eba628a3a294d05f0193467b629efc4c/Screenshots/01%20-%20Alias.JPG)


```SQL
-- Return all columns, restricting the N percent of rows returned
SELECT 
  TOP (50) PERCENT *
FROM 
  eurovision;
```
![TOP N %](https://github.com/jaume-rsl/SQL/blob/12858852eba628a3a294d05f0193467b629efc4c/Screenshots/02%20-%20Top%20N%20percent.JPG)
