# SQL Examples

Useful SQL queries for doing interesting things.
Each query should be accompanied by an explanation of its purpose
Viewing the latest data
View the last 100 rows in the database (assuming id is an ascending (or auto-incremental) value

## Database management


## Schema management


## Query data

```sql
SELECT * FROM table ORDER BY id DESC LIMIT 100
```

Select the last 100 rows in a table (assuming id is an auto-incremental index) and order the results in ascending order

```sql
SELECT * FROM (
  SELECT * FROM table ORDER BY id DESC LIMIT 100
) sub
ORDER BY id ASC
```

