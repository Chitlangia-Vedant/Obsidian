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
# SUM OVER/ AVG OVER
# 
You can avoid the `IN` subqueries by using `COUNT() OVER()`:

SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016

FROM (

    SELECT *,

           COUNT(*) OVER (PARTITION BY tiv_2015) AS tiv_count,

           COUNT(*) OVER (PARTITION BY lat, lon) AS location_count

    FROM Insurance

) AS x

WHERE tiv_count > 1

  AND location_count = 1;

This is often easier to understand:

COUNT(*) OVER (PARTITION BY tiv_2015)

        ↓

How many policies have this tiv_2015?

  

COUNT(*) OVER (PARTITION BY lat, lon)

        ↓

How many policies have this location?

  

tiv_count > 1

        AND

location_count = 1

Your original solution is completely fine, though. For LeetCode, I'd consider it a **clean and standard solution**.

# DENSE_RANK()
# GROUP_CONCATE()
# REGEXP