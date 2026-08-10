**The `SELECT` statement is used to select data from a database.**
## Syntax

```
SELECT column1, column2, ...  
FROM table_name;
```
### Example

Return data from the Customers table:
```
SELECT CustomerName, City FROM Customers;
```
## Select ALL Columns

To select ALL columns, without specifying every column name, use the `SELECT *` syntax:
### Example

Select ALL columns from the "Customers" table:
```
SELECT * FROM Customers;
```

# SELECT DISTINCT
**The `SELECT DISTINCT` statement is used to return only distinct (unique) values.**

In a table, a column may contain several duplicate values - and sometimes you want to list only the unique values.
### Example
Select all the distinct (unique) countries from the "Customers" table:

```
SELECT DISTINCT Country FROM Customers;
```
## Syntax

```
SELECT DISTINCT column1, column2, ...   
FROM table_name;
```
## Count Distinct Values
By using the `COUNT()` function with the `DISTINCT` keyword, we can count the number of unique countries.
### Example

```
SELECT COUNT(DISTINCT Country) 
FROM Customers;
```