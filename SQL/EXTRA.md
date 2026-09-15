# 

```
COUNT(CASE WHEN rating < 3 THEN 1 END)
```

```
SUM(rating < 3)
```

# 
WITH RECURSIVE triangle AS (
    SELECT 20 AS n

    UNION ALL

    SELECT n - 1
    FROM triangle
    WHERE n > 1
)
SELECT REPEAT('* ', n)
FROM triangle;
