# Database 2: More SQL and integration with NodeJS

## What will we learn today?
- Revision from last week
- More SQL
- Using aggregate functions in a query
- Inserting, Updating and Deleting Rows
- Joining tables
- Other useful operations
- Creating a Table
- Integration with NodeJS
- Introduction to node-postgres
- Loading data from a database with a GET endpoint


## Learning Objectives
- Aggregate data over multiple rows and use aggregate values to restrict the results
- Insert, Update and Delete rows in a pre-existing table using INSERT, UPDATE and DELETE
- Combine tables together using INNER JOIN
- Create a table with primary and foreign keys
- Connect a PostgreSQL database to a NodeJS application
- Retrieve data from a PostgreSQL database in a NodeJS application

## Note:
### Aggregate functions
Aggregate functions are functions in a database management system (DBMS) that operate on a set of values and return a single calculated value as a result. They are used to perform calculations and summarizations on groups of rows or columns in a database table. Here are some commonly used aggregate functions:

- COUNT: Returns the number of rows that match a specified condition or the total number of rows in a table.

- SUM: Calculates the sum of all values in a numeric column.

- AVG: Calculates the average (mean) value of a numeric column.

- MIN: Returns the minimum value from a column.

- MAX: Returns the maximum value from a column.

- GROUP_CONCAT: Concatenates values from multiple rows into a single string,
with an optional separator.

- DISTINCT: Returns only distinct (unique) values from a column.

- STDEV/STDEVP: Calculates the standard deviation of a column (STDEV) or the standard deviation of a population (STDEVP).

- VAR/VARP: Calculates the variance of a column (VAR) or the variance of a population (VARP).

- FIRST/LAST: Returns the first or last value in a column, based on the order specified.

These functions are typically used in conjunction with the GROUP BY clause to group rows based on one or more columns. By using aggregate functions, you can summarize and extract meaningful insights from large datasets in a database.


# Using Aggregate Functions
## Basic Aggregate Functions
### How to calculate totals, averages, etc. over multiple rows.

You frequently need to get a single piece of information that is derived from multiple rows in a table. For example, when you need to know the total of all invoices for August 2018:
```
SELECT sum(total)
   FROM invoices
   WHERE invoice_date BETWEEN
         '2018-08-01' AND '2018-08-31';
```
The aggregate functions are:

- sum : Calculate the total of the values in a column
- avg : Calculate the average (mean) of the values in a column
- min : Determine the mimimum value of a column
- max : Determine the maximum value of a column
- count : Count the number of values (non-null) in a column

All the above are in the SQL standard, most implementations provide others. SUM and AVG can only apply to numeric data, the others can apply to any datatype.

Further examples:

"What is the average length of stay at our hotel?" :

```
SELECT avg(checkout_date - checkin_date)
  FROM reservations;
```  
What are the lowest and highest room rates we charge?" :

```
SELECT min(rate) AS lowest,
       max(rate) AS highest
  FROM rooms;
```
You can use the count(x) function to count non-null values:

```
SELECT count(id) AS id_ct, count(postcode) AS post_ct
  FROM customers;

   id_ct | post_ct
  -------+---------
     133 |     126
  (1 row)
  ```
Notice that these two results show different values - there are NULL values for postcode but id is mandatory for all rows.

If you just want to count the number of rows, use count(*). This is often used to find how many rows match a WHERE clause:

### Exercise 1
1. Get the numbers of rows in each of the tables: rooms, room_types, customers and reservations.
```
SELECT COUNT(*) AS row_count FROM rooms;
SELECT COUNT(*) AS room_type_count FROM room_types;
SELECT COUNT(*) AS customer_count FROM customers;
SELECT COUNT(*) AS reservation_count FROM reservations;
```
How many reservations do we have for next month?
````
SELECT COUNT(*) AS reservation_count
FROM reservations
WHERE reservation_date >= DATEADD(MONTH, 1, DATEADD(DAY, 1, EOMONTH(GETDATE())))
  AND reservation_date < DATEADD(MONTH, 2, DATEADD(DAY, 1, EOMONTH(GETDATE())));
```
How many invoices are still unpaid from over a month ago and what is the total owed?
What is the maximum gap in days between a customer booking a room and the checkin date for that booking?

