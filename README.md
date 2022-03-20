# database-tasks

My Guitar Shop Exercises 2 for Murach’s MySQL (3rd Edition)
Task 1 
In these exercises, you’ll enter and run your own SELECT statements.
1. Write a SELECT statement that returns four columns from the Products table:
product_code, product_name, list_price, and discount_percent. Then, run this
statement to make sure it works correctly.
Add an ORDER BY clause to this statement that sorts the result set by list price in
descending sequence. Then, run this statement again to make sure it works correctly.
This is a good way to build and test a statement, one clause at a time.

USE my_guitar_shop;

-- 1.---------------------------------
SELECT product_code,
       product_name,
       list_price,
       discount_percent
FROM   products
ORDER  BY list_price DESC;


2. Write a SELECT statement that returns one column from the Customers table named
full_name that joins the last_name and first_name columns.
Format this column with the last name, a comma, a space, and the first name like this:
Doe, John
Sort the result set by the last_name column in ascending sequence.
Return only the customers whose last name begins with letters from M to Z.

-- 2. ----------------------------------
SELECT Concat(last_name, ',  ', first_name) AS full_name
FROM   customers
-- There are 2 ways to start the names with M
-- 1st WAY -----    
-- WHERE last_name >= 'M'  
-- 2nd WAY ------  
WHERE  last_name REGEXP '^[M-Z]'
ORDER  BY last_name ASC;


3. Write a SELECT statement that joins the Categories table to the Products table and
returns these columns: category_name, product_name, list_price.
Sort the result set by the category_name column and then by the product_name
column in ascending sequence.
Return only the rows with a list price that’s greater than 500 and less than 2000.
Use the LIMIT clause so the result set contains only the first 5 rows.

-- 3. -----------------------------------
SELECT c.category_name,
       p.product_name,
       p.list_price
FROM   categories c
       JOIN products p
         ON c.category_id = p.category_id
WHERE  list_price > 500
       AND list_price < 2000
ORDER  BY c.category_name,
          p.product_name
LIMIT  5;

4. Write a SELECT statement that joins the Customers, Orders, Order_Items, and
Products tables. This statement should return these columns: last_name, first_name,
order_date, product_name, item_price, discount_amount, and quantity.
Use aliases for the tables.
Sort the final result set by the last_name, order_date, and product_name columns.

-- 4. ------------------------------
SELECT c.last_name,
       c.first_name,
       o.order_date,
       p.product_name,
       i.item_price,
       i.discount_amount,
       i.quantity
FROM   customers AS c
       JOIN orders AS o
         ON c.customer_id = o.customer_id
       JOIN order_items AS i
         ON o.order_id = i.order_id
       JOIN products AS p
         ON i.product_id = p.product_id
ORDER  BY c.last_name,
          o.order_date,
          p.product_name;
          
          
5. Use the UNION operator to generate a result set consisting of three columns from the
Orders table:
ship_status A calculated column that contains a value of
SHIPPED or NOT SHIPPED
order_id The order_id column
order_date The order_date column
If the order has a value in the ship_date column, the ship_status column should
contain a value of SHIPPED. Otherwise, it should contain a value of NOT SHIPPED.
Sort the final result set by the order_date column.

-- 5. --------------------------------
SELECT 'SHIPPED' AS ShipStatus,
       order_id,
       order_date
FROM   orders
WHERE  ship_date IS NOT NULL
UNION
SELECT 'NOT SHIPPED',
       order_id,
       order_date
FROM   orders
WHERE  ship_date IS NULL
ORDER  BY order_date;


6. Write a SELECT statement that returns these columns from the Orders table:
order_id The order_id column
order_date The order_date column
ship_date The ship_date column
Return only the rows where the ship_date column contains a null value.

-- 6. ----------------------------------
SELECT order_id   AS 'The order id',
       order_date AS 'The order date',
       ship_date  AS 'The ship date'
FROM   orders
WHERE  ship_date IS NULL;


7. Write a SELECT statement without a FROM clause that creates a row with these
columns:
price 100 (dollars)
tax_rate .07 (7 percent)
tax_amount The price multiplied by the tax
total The price plus the tax
To calculate the fourth column, add the expressions you used for the first and third
columns.

-- 7. ----------------------------------------
SELECT 100                     AS Price,
       .07                     AS Tax_Rate,
       100 * .07               AS Tax_Amount,
       ( 100 ) + ( 100 * .07 ) AS Total;
-- ---------------------------------------------
