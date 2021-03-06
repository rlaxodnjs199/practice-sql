# Practice SQL
## 1. Returning Strings

:scroll: Problem

Write a select statement that takes name from person table and return "Hello, <name> how are you doing today?" results in a column named greeting

:mag_right: Example

```
name = "John" -> greeting = "Hello, John how are you doing today?"
```  

:rocket: Solution
  
```sql
SELECT format('Hello, %s how are you doing today?', name)
AS greeting
FROM person;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 2. On the Canadian Border

:scroll: Problem

You are a border guard sitting on the Canadian border. You were given a list of travelers who have arrived at your gate today. You know that American, Mexican, and Canadian citizens don't need visas, so they can just continue their trips. You don't need to check their passports for visas! You only need to check the passports of citizens of all other countries!

Select names, and countries of origin of all the travelers, excluding anyone from Canada, Mexico, or The US.

travelers table schema

name
country
NOTE: The United States is written as 'USA' in the table.

:rocket: Solution
  
```sql
SELECT name, country
FROM travelers
WHERE lower(country) NOT IN ('usa', 'canada', 'mexico')
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 3. Register for the Party

:scroll: Problem

You received an invitation to an amazing party. Now you need to write an insert statement to add yourself to the table of participants.

participants table schema

name (string)
age (integer)
attending (boolean)
NOTES:

Since alcohol will be served, you can only attend if you are 21 or older
You can't attend if the attending column returns anything but true

:rocket: Solution
  
```sql
INSERT INTO participants (name, age, attending) VALUES ('twkim', 31, true);
--INSERT INTO participants VALUES ('twkim', 31, true); -> Can omit columns info when inserting all values for all columns in correct order
SELECT * FROM participants
```
  
---

**[⬆ Back to Top](#practice-sql)**
  
## 3. Collect Tuition

:scroll: Problem

You are working for a local school, and you are responsible for collecting tuition from students. You have a list of all students, some of them have already paid tution and some haven't. Write a select statement to get a list of all students who haven't paid their tuition yet. The list should include all the data available about these students.

students table schema

name (string)
age (integer)
semester (integer)
mentor (string)
tuition_received (Boolean)

:rocket: Solution
  
```sql
SELECT *
FROM students
WHERE NOT tuition_received
--Or WHERE tuition_received = False
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 4. Best-Selling Books

:scroll: Problem

You work at a book store. It's the end of the month, and you need to find out the 5 bestselling books at your store. Use a select statement to list names, authors, and number of copies sold of the 5 books which were sold most.

books table schema

name
author
copies_sold

:rocket: Solution
  
```sql
SELECT * FROM books ORDER BY copies_sold DESC LIMIT 5;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 5. Countries Capitals for Trivia Night

:scroll: Problem

Your friends told you that if you keep coding on your computer, you are going to hurt your eyes. They suggested that you go with them to trivia night at the local club.

Once you arrive at the club, you realize the true motive behind your friends' invitation. They know that you are a computer nerd, and they want you to query the countries table and get the answers to the trivia questions.

Schema of the countries table:

country (String)
capital (String)
continent (String)
The first question: from the African countries that start with the character E, get the names of their capitals ordered alphabetically.

You should only give the names of the capitals. Any additional information is just noise
If you get more than 3, you will be kicked out, for being too smart
Also, this database is crowd-sourced, so sometimes Africa is written Africa and in other times Afrika.

:rocket: Solution
  
```sql
SELECT capital 
FROM countries 
WHERE continent IN ('Africa', 'Afrika') 
  AND country LIKE 'E%' 
ORDER BY capital
LIMIT 3;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 6. Even or Odd

:scroll: Problem

You will be given a table, numbers, with one column number.

Return a table with a column is_even containing "Even" or "Odd" depending on number column values.

numbers table schema
number INT
output table schema
is_even STRING

:rocket: Solution
  
```sql
SELECT
  CASE
    WHEN number % 2 = 0 THEN 'Even'
    ELSE 'Odd'
  END AS is_even
FROM numbers;
```
  
---

**[⬆ Back to Top](#practice-sql)**
  
## 7. Create a FUNCTION

:scroll: Problem

For this challenge you need to create a basic Increment function which Increments on the age field of the peoples table.

The function should be called increment, it needs to take 1 integer and increment that number by 1.

:rocket: Solution
  
```sql
CREATE FUNCTION increment(age integer) RETURNS integer AS $$
BEGIN
  RETURN age + 1;
END;
$$ LANGUAGE plpgsql
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 8. Simple VIEW

:scroll: Problem

For this challenge you need to create a VIEW. This VIEW is used by a sales store to give out vouches to members who have spent over $1000 in departments that have brought in more than $10000 total ordered by the members id. The VIEW must be called members_approved_for_voucher then you must create a SELECT query using the view.

![image](https://user-images.githubusercontent.com/26533193/123575152-c5e33480-d796-11eb-89e4-d3db074a32ed.png)

:rocket: Solution
  
```sql
CREATE VIEW members_approved_for_voucher AS
SELECT m.id, m.name, m.email, SUM(p.price) AS "total_spending"
FROM members m
INNER JOIN sales s ON s.member_id = m.id
INNER JOIN products p ON p.id = s.product_id
WHERE s.department_id IN (
  SELECT s2.department_id
  FROM sales s2
  INNER JOIN products p2 ON p2.id = s2.product_id
  GROUP BY s2.department_id
  HAVING SUM(p2.price) > 10000
)
GROUP BY m.id
HAVING SUM(p.price) > 1000
ORDER BY m.id;

SELECT * FROM members_approved_for_voucher;
```
  
---

**[⬆ Back to Top](#practice-sql)**
  
## 9. GROCERY STORE: Support Local Products

:scroll: Problem
You are the owner of the Grocery Store. All your products are in the database, that you have created after CodeWars SQL excercises!:)

You care about local market, and want to check how many products come from United States of America or Canada.

Please use SELECT statement and IN to filter out other origins.

In the results show how many products are from United States of America and Canada respectively.

Order by number of products, descending.

products table schema
id
name
price
stock
producer
country
  
results table schema
products
country

:rocket: Solution
  
```sql
CREATE count(*) AS products, country
FROM products
WHERE country IN ('United States of America', 'Canada')
GROUP BY country
ORDER BY count(*) DESC;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 10. SQL Basics: Simple FULL TEXT SEARCH

:scroll: Problem
  
For this challenge you need to create a simple SELECT statement. Your task is to create a query and do a FULL TEXT SEARCH. You must search the product table on the field name for the word Awesome and return each row with the given word. Your query MUST contain to_tsvector and to_tsquery PostgreSQL functions.

:rocket: Solution
  
```sql
SELECT * FROM products
WHERE name ~ 'Awesome'
```
  
---

**[⬆ Back to Top](#practice-sql)**
  
## 11. SQL Statistics: MIN, MEDIAN, MAX

:scroll: Problem
  
For this challenge you need to create a simple SELECT statement. Your task is to calculate the MIN, MEDIAN and MAX scores of the students from the results table.

:rocket: Solution
  
```sql
SELECT
  MIN(score) AS min,
  percentile_cont(0.5) WITHIN GROUP (ORDER BY score) AS median, 
  MAX(score)
FROM result;
```
  
---

**[⬆ Back to Top](#practice-sql)**

## 12. Relational division: Find all movies two actors cast in together

:scroll: Problem
  
Given film_actor and film tables from the DVD Rental sample database find all movies both Sidney Crowe (actor_id = 105) and Salma Nolte (actor_id = 122) cast in together and order the result set alphabetically.

```
film schema
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
title       | character varying(255)      | not null
film_id     | smallint                    | not null

film_actor schema
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
actor_id    | smallint                    | not null
film_id     | smallint                    | not null
last_update | timestamp without time zone | not null 

actor schema
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
actor_id    | integer                     | not null 
first_name  | character varying(45)       | not null
last_name   | character varying(45)       | not null
last_update | timestamp without time zone | not null 
The desired output:

title
-------------
Film Title 1
Film Title 2
...
```

:rocket: Solution
  
```sql
SELECT f.title
FROM film f
JOIN film_actor fa ON fa.film_id = f.film_id
WHERE fa.actor_id IN (105, 122)
GROUP BY f.film_id
HAVING COUNT(*) = 2
ORDER BY f.title
```

```sql
SELECT f.title
FROM film f
INNER JOIN film_actor fa1 ON fa1.film_id = f.film_id
INNER JOIN film_actor fa2 ON fa2.film_id = f.film_id
WHERE fa1.film_id = 105 AND fa2.film_id = 122
ORDER BY f.title
```
---

**[⬆ Back to Top](#practice-sql)**

## 13. Calculating Running Total

:scroll: Problem
  
Description
Given a posts table that contains a created_at timestamp column write a query that returns date (without time component), a number of posts for a given date and a running (cumulative) total number of posts up until a given date. The resulting set should be ordered chronologically by date.

Desired Output
The resulting set should look similar to the following:

```
date       | count | total
-----------+-------+-------
2017-01-26 |    20 |    20
2017-01-27 |    17 |    37
2017-01-28 |     7 |    44
2017-01-29 |     8 |    52
...
```
date - (DATE) date
count - (INT) number of posts for a date
total - (INT) a running (cumulative) number of posts up until a date

:rocket: Solution
  
```sql
SELECT 
  created_at::date AS date, 
  count(*), 
  SUM(COUNT(*)) OVER (ORDER BY created_at::date ROWS UNBOUNDED PRECEDING)::int AS total
FROM posts
GROUP BY date
ORDER BY date;
```
---

**[⬆ Back to Top](#practice-sql)**
  
## 14. Calculating Month-Over-Month Percentage Growth Rate

:scroll: Problem

Given a posts table that contains a created_at timestamp column write a query that returns a first date of the month, a number of posts created in a given month and a month-over-month growth rate.

The resulting set should be ordered chronologically by date.

Note:

percent growth rate can be negative
percent growth rate should be rounded to one digit after the decimal point and immediately followed by a percent symbol "%". See the desired output below for the reference.
Desired Output
The resulting set should look similar to the following:

```
date       | count | percent_growth
-----------+-------+---------------
2016-02-01 |   175 |           null
2016-03-01 |   338 |          93.1%
2016-04-01 |   345 |           2.1%
2016-05-01 |   295 |         -14.5%
2016-06-01 |   330 |          11.9%
...
```
date - (DATE) a first date of the month
count - (INT) a number of posts in a given month
percent_growth - (TEXT) a month-over-month growth rate expressed in percents
:rocket: Solution
  
```sql
WITH temp AS (
  SELECT
    to_char(created_at::date, 'YYYY-MM-01')::date AS date,
    COUNT(*) AS count
  FROM posts
  GROUP BY date
  ORDER BY date
)

SELECT 
  date,
  count,
  to_char((count - LAG(count) OVER ()) / LAG(count) OVER ()::float * 100, 'FM990.0%') AS percent_growth
FROM temp;
```
---

**[⬆ Back to Top](#practice-sql)**

## 15. SQL Basics: Simple PIVOTING data WITHOUT CROSSTAB

:scroll: Problem

You need to build a pivot table WITHOUT using CROSSTAB function. Having two tables products and details you need to select a pivot table of products with counts of details occurrences (possible details values are ['good', 'ok', 'bad'].

Results should be ordered by product's name.

Model schema for the kata is:

![image](https://user-images.githubusercontent.com/26533193/125149005-e5198480-e0fb-11eb-860b-486f5c16fd5f.png)

schema

your query should return table with next columns

name
good
ok
bad
Compare your table to the expected table to view the expected results.

:rocket: Solution
  
```sql
SELECT
  p.name,
  count(CASE WHEN d.detail = 'good' THEN 1 END) AS good,
  count(CASE WHEN d.detail = 'ok' THEN 1 END) AS ok,
  count(CASE WHEN d.detail = 'bad' THEN 1 END) AS bad
FROM products p
INNER JOIN details d ON d.product_id = p.id
GROUP BY p.name
ORDER BY p.name;
```
---

**[⬆ Back to Top](#practice-sql)**

## 16. SQL Basics: Top 10 customers by total payments amount

:scroll: Problem

Overview
For this kata we will be using the DVD Rental database.

Your are working for a company that wants to reward its top 10 customers with a free gift. You have been asked to generate a simple report that returns the top 10 customers by total amount spent ordered from highest to lowest. Total number of payments has also been requested.

The query should output the following columns:

customer_id [int4]
email [varchar]
payments_count [int]
total_amount [float]
and has the following requirements:

only returns the 10 top customers, ordered by total amount spent from highest to lowest
  
Database Schema
![image](https://user-images.githubusercontent.com/26533193/125180718-f8435780-e1c2-11eb-8ca3-8444fc982e1d.png)

:rocket: Solution
  
```sql
SELECT
  c.customer_id,
  email,
  COUNT(*) AS payments_count,
  SUM(p.amount)::float AS total_amount
FROM customer c
INNER JOIN payment p ON p.customer_id = c.customer_id
GROUP BY c.customer_id
ORDER BY total_amount DESC
LIMIT 10;
```
---

**[⬆ Back to Top](#practice-sql)**

## 17. Using Window Functions To Get Top N per Group

:scroll: Problem

Given the schema presented below write a query, which uses a window function, that returns two most viewed posts for every category.

Order the result set by:

category name alphabetically
number of post views largest to lowest
post id lowest to largest
Note:
Some categories may have less than two or no posts at all.
Two or more posts within the category can be tied by (have the same) the number of views. Use post id as a tie breaker - a post with a lower id gets a higher rank.
Schema
categories
```
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
id          | integer                     | not null
category    | character varying(255)      | not null
```
posts
```
  Column     | Type                        | Modifiers
------------+-----------------------------+----------
id          | integer                     | not null
category_id | integer                     | not null
title       | character varying(255)      | not null
views       | integer                     | not null
```
Desired Output
The desired output should look like this:
```
category_id | category | title                             | views | post_id
------------+----------+-----------------------------------+-------+--------
5           | art      | Most viewed post about Art        | 9234  | 234
5           | art      | Second most viewed post about Art | 9234  | 712
2           | business | NULL                              | NULL  | NULL
7           | sport    | Most viewed post about Sport      | 10    | 126
...
```
```
category_id - category id
category - category name
title - post title
views - the number of post views
post_id - post id
```
:rocket: Solution

```sql
WITH temp AS (
  SELECT 
    c.id AS category_id,
    c.category,
    p.title,
    p.views AS views,
    p.id AS post_id,
    ROW_NUMBER() OVER (PARTITION BY p.category_id ORDER BY p.views DESC, p.id) rows_num
  FROM posts p
  RIGHT JOIN categories c ON p.category_id = c.id
)

SELECT 
  category_id,
  category,
  title,
  views,
  post_id
FROM temp
WHERE rows_num <= 2
ORDER BY category, views DESC, post_id;
```
---

**[⬆ Back to Top](#practice-sql)**

## 18. SQL Basics: Simple table totaling

:scroll: Problem

For this challenge you need to create a simple query to display each unique clan with their total points and ranked by their total points.

people table schema
```
name
points
clan
```                  
You should then return a table that resembles below

select on
```
rank
clan
total_points
total_people
```
The query must rank each clan by their total_points, you must return each unqiue clan and if there is no clan name (i.e. it is an empty string) you must replace it with [no clan specified], you must sum the total_points for each clan and the total_people within that clan.

##Note The data is loaded from the live leaderboard, this means values will change but also could cause the kata to time out retreiving the information.

:rocket: Solution

```sql
SELECT
  RANK() OVER (ORDER BY SUM(points) DESC),
  COALESCE(NULLIF(clan, ''), '[no clan specified]') AS clan,
  SUM(points) AS total_points,
  COUNT(*) AS total_people
FROM people
GROUP BY clan
ORDER BY total_points DESC;
```
---

**[⬆ Back to Top](#practice-sql)**

## 19. SQL Basics: Simple WITH

:scroll: Problem

For this challenge you need to create a SELECT statement, this SELECT statement will use an IN to check whether a department has had a sale with a price over 90.00 dollars BUT the sql MUST use the WITH statement which will be used to select all columns from sales where the price is greater than 90.00, you must call this sub-query special_sales.

departments table schema
```
id
name
```
sales table schema
```
id
department_id (department foreign key)
name
price
card_name
card_number
transaction_date
```  
resultant table schema
```
id
name
```

:rocket: Solution

```sql
WITH special_sales AS 
  (SELECT * FROM sales WHERE price > 90.00)

SELECT * FROM departments
WHERE id IN (SELECT department_id FROM special_sales);
```
---

**[⬆ Back to Top](#practice-sql)**

## 20. SQL Basics: Simple NULL handling

:scroll: Problem

For this challenge you need to create a SELECT statement, this select statement must have NULL handling, using COALESCE and NULLIF.

If no name is specified you must replace with [product name not found]

If no card_name is specified you must replace with [card name not found]

If no price is specified you must throw away the record, you must also filter the dataset by prices greater than 50.

eusales table schema
```
id
name
price
card_name
card_number
transaction_date
```
resultant table schema
```
id
name
price (greater than 50.00)
card_name
card_number
transaction_date
```

:rocket: Solution

```sql
SELECT
  id,
  COALESCE(NULLIF(name, ''), '[product name not found]') AS name,
  price,
  COALESCE(NULLIF(card_name, ''), '[card name not found]') AS card_name,
  card_number,
  transaction_date
FROM eusales
WHERE price > 50;
```
---

**[⬆ Back to Top](#practice-sql)**

## 21. SQL Basics: Simple EXISTS

:scroll: Problem

For this challenge you need to create a SELECT statement that will contain data about departments that had a sale with a price over 98.00 dollars. This SELECT statement will have to use an EXISTS to achieve the task.

departments table schema
```
id
name
```
sales table schema
```
id
department_id (department foreign key)
name
price
card_name
card_number
transaction_date
```
resultant table schema
```
id
name
```

:rocket: Solution

```sql
SELECT * FROM departments
WHERE EXISTS (SELECT 1 FROM sales WHERE department_id = departments.id AND price > 98)
```
---

**[⬆ Back to Top](#practice-sql)**

## 22. Present JSON data the SQL way

:scroll: Problem

Task
Given the database where users are stored in JSON format, fetch the records splitting the data into separate columns.

Notes
The private field determines whether the user's email address should be publicly visible
If the profile is private, email_address should equal "Hidden"
The users may have multiple email addresses
If no email addresses are provided, email_address should equal "None"
If there're multiple email addresses, the first one should be shown
The date_of_birth is in the yyyy-mm-dd format
The age fields represents the user's age in years
Order the result by the first_name, and last_name columns
```
Input table
-------------------------
| Table | Column | Type |
|-------+--------+------|
| users | id     | int  |
|       | data   | json |
-------------------------
```
```
JSON object format
--------------------------------------
|     Field       |       Type       |
|-----------------+------------------|
| first_name      | string           |
| last_name       | string           |
| date_of_birth   | string           |
| email_addresses | array of strings |
| private         | boolean          |
--------------------------------------
```
```
Output table
------------------------
|    Column     | Type |
|---------------+------|
| first_name    | text |
| last_name     | text |
| age           | int  |
| email_address | text |
------------------------  
```
:rocket: Solution

```sql
SELECT
  data->>'first_name' AS first_name,
  data->>'last_name' AS last_name,
  EXTRACT(year FROM age(now(), (data->>'date_of_birth')::date))::integer AS age,
  CASE
    WHEN data->>'private' = 'true' THEN 'Hidden'
    WHEN data->>'private' = 'false' AND data#>>'{email_addresses, 0}' IS NULL THEN 'None'
    ELSE data#>>'{email_addresses, 0}'
  END AS email_address
FROM users
ORDER BY first_name, last_name;
```
---

**[⬆ Back to Top](#practice-sql)**

## 23. SQL Data: Company Data - totals per day

:scroll: Problem

Your task is simple, given the data you must calculate on each day how many hours have been clocked up for each department.

Tables and relationship below:
![image](https://user-images.githubusercontent.com/26533193/126087371-7290c67a-e7b7-43db-8602-454c2baf3944.png)
  
Resultant table:
```
day (date) [order by]
department_name (name of department) [order by]
total_hours (total hours for the day)
```

:rocket: Solution

```sql
SELECT
  login::DATE AS day,
  name AS department_name,
  SUM(EXTRACT(hour FROM AGE(logout, login))) AS total_hours
FROM timesheet t
INNER JOIN department d ON d.id = t.department_id
GROUP BY day, department_name
ORDER BY day, department_name;
```
---

**[⬆ Back to Top](#practice-sql)**

## 24. SQL Basics: Simple IN

:scroll: Problem

For this challenge you need to create a SELECT statement, this SELECT statement will use an IN to check whether a department has had a sale with a price over 98.00 dollars.

departments table schema
```
id
name
```
sales table schema
```
id
department_id (department foreign key)
name
price
card_name
card_number
transaction_date
```
resultant table schema
```
id
name
```

:rocket: Solution

```sql
SELECT * FROM departments
WHERE id IN (SELECT department_id FROM sales WHERE price > 98.00)
```
---

**[⬆ Back to Top](#practice-sql)**

## 25. SQL Basics: Simple JOIN and RANK

:scroll: Problem

For this challenge you need to create a simple SELECT statement that will return all columns from the people table, and join to the sales table so that you can return the COUNT of all sales and RANK each person by their sale_count.

people table schema
```
id
name
```
sales table schema
```
id
people_id
sale
price
```
You should return all people fields as well as the sale count as "sale_count" and the rank as "sale_rank".

:rocket: Solution

```sql
SELECT
  p.*,
  COUNT(*) AS sale_count,
  RANK() OVER (ORDER BY COUNT(*)) AS sale_rank
FROM people p
INNER JOIN sales s ON s.people_id = p.id
GROUP BY p.id;
```
---

**[⬆ Back to Top](#practice-sql)**
  
## 25. SQL Basics: Simple JOIN and RANK

:scroll: Problem

Oh no! Timmys been moved into the database divison of his software company but as we know Timmy loves making mistakes. Help Timmy keep his job by fixing his query...

Timmy works for a statistical analysis company and has been given a task of calculating the highest average salary for a given job, the sample is compiled of 100 applicants each with a job and a salary. Timmy must display each unique job, the total average salary, the total people and the total salary and order by highest average salary. Timmy has some bugs in his query, help Timmy fix his query so he can keep his job!

people table schema
```
id
name
```
job table schema
```
id
people_id
job_title
salary
```
resultant table schema
```
job_title (unique)
average_salary (float, 2 dp)
total_people (int)
total_salary (float, 2 dp)
```

:rocket: Solution

```sql
SELECT
  j.job_title,
  ROUND(AVG(j.salary), 2)::float AS average_salary,
  COUNT(*) AS total_people,
  ROUND(SUM(j.salary), 2)::float AS total_salary
FROM job j
INNER JOIN people p on p.id = j.people_id
GROUP BY j.job_title
ORDER BY average_salary;
```
---

**[⬆ Back to Top](#practice-sql)**
  
## 26. Conditional Count

:scroll: Problem

Given a payment table, which is a part of DVD Rental Sample Database, with the following schema
```
Column       | Type                        | Modifiers
-------------+-----------------------------+----------
payment_id   | integer                     | not null 
customer_id  | smallint                    | not null
staff_id     | smallint                    | not null
rental_id    | integer                     | not null
amount       | numeric(5,2)                | not null
payment_date | timestamp without time zone | not null
```
produce a result set for the report that shows a side-by-side comparison of the number and total amounts of payments made in Mike's and Jon's stores broken down by months.

The resulting data set should be ordered by month using natural order (Jan, Feb, Mar, etc.).

Note: You don't need to worry about the year component. Months are never repeated because the sample data set contains payment information only for one year.

The desired output for the report
```
month | total_count | total_amount | mike_count | mike_amount | jon_count | jon_amount
------+-------------+--------------+------------+-------------+-----------+-----------
2     |             |              |            |             |           |           
5     |             |              |            |             |           |           
...
```
month - number of the month (1 - January, 2 - February, etc.)
total_count - total number of payments
total_amount - total payment amount
mike_count - total number of payments accepted by Mike (staff_id = 1)
mike_amount - total amount of payments accepted by Mike (staff_id = 1)
jon_count - total number of payments accepted by Jon (staff_id = 2)
jon_amount - total amount of payments accepted by Jon (staff_id = 2)

:rocket: Solution

```sql
SELECT
  EXTRACT(MONTH FROM payment_date) AS month,
  COUNT(*) AS total_count,
  SUM(amount) AS total_amount,
  COUNT(*) FILTER (WHERE staff_id = 1) AS mike_count,
  SUM(amount) FILTER (WHERE staff_id = 1) AS mike_amount,
  COUNT(*) FILTER (WHERE staff_id = 2) AS jon_count,
  SUM(amount) FILTER (WHERE staff_id = 2) AS jon_amount
FROM payment
GROUP BY month
ORDER BY month;
```
---

**[⬆ Back to Top](#practice-sql)**
  
## 27. SQL Basics: Simple Hierarchical structure

:scroll: Problem

For this challenge you need to create a RECURSIVE Hierarchical query. You have a table employees of employees, you must order each employee by level. You must use a WITH statement and name it employee_levels after that has been defined you must select from it.

A Level is in correlation what manager managers the employee. e.g. an employee with a manager_id of NULL is at level 1 and then direct employees with the employee at level 1 will be level 2.

employees table schema
```
id
first_name
last_name
manager_id (can be NULL)
```
resultant schema
```
level
id
first_name
last_name
manager_id (can be NULL)
```

:rocket: Solution

```sql
WITH RECURSIVE employee_levels AS (
  SELECT 1 AS level, *
  FROM employees
  WHERE manager_id is NULL
  
  UNION ALL
  
  SELECT el.level + 1, e.*
  FROM employees e, employee_levels el
  WHERE e.manager_id = el.id
)
  
SELECT * FROM employee_levels;
```
---

**[⬆ Back to Top](#practice-sql)**
  
## 28. SQL Basics - Monsters using CASE

:scroll: Problem
  
You have access to two tables named top_half and bottom_half, as follows:

top_half schema
```
id
heads
arms
```
bottom_half schema
```
id
legs
tails
```
You must return a table with the format as follows:

output schema
```
id
heads
legs
arms
tails
species
```
The IDs on the tables match to make a full monster. For heads, arms, legs and tails you need to draw in the data from each table.

For the species, if the monster has more heads than arms, more tails than legs, or both, it is a 'BEAST' else it is a 'WEIRDO'. This needs to be captured in the species column.

All rows should be returned (10).

Tests require the use of CASE. Order by species.

:rocket: Solution

```sql
SELECT
  t.*,
  b.*,
  CASE WHEN t.heads > t.arms OR b.tails > b.legs
    THEN 'BEAST'
    ELSE 'WEIRDO'
  END AS species
FROM top_half t
INNER JOIN bottom_half b ON b.id = t.id
ORDER BY species;
```
---

**[⬆ Back to Top](#practice-sql)**
