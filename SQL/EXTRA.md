# Count how many rows have `rating < 3`

```
COUNT(CASE WHEN rating < 3 THEN 1 END)
```

```
SUM(rating < 3)
```
