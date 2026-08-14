# DATE_SUB
# DATEDIFF
# ROUND
# COALESCE
# 
No, `COUNT(* WHERE rating < 3)` is **not valid SQL**.

Use **conditional aggregation**:

COUNT(CASE WHEN rating < 3 THEN 1 END)

Or in MySQL, a simpler form:

SUM(rating < 3)

### Example

SELECT

    COUNT(*) AS total,

    COUNT(CASE WHEN rating < 3 THEN 1 END) AS low_ratings

FROM Reviews;

You can also use:

SUM(rating < 3) AS low_ratings

because MySQL treats `TRUE` as `1` and `FALSE` as `0`.
#