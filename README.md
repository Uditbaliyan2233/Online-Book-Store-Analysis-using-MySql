# Online-Book-Store-Analysis-using-MySql
A MySQL data analysis project on an Online Book Store database demonstrating SQL queries, joins, aggregation, customer insights, sales analysis, and inventory management.


CREATE DATABASE ONLINE_BOOK_STORE;
use online_book_store;
CREATE TABLE books (
    Book_id SERIAL,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(100),
    Published VARCHAR(100),
    Price DECIMAL(10 , 2 ),
    Orders INT
);
alter table books add primary key(book_id);
ALTER TABLE books
RENAME COLUMN Orders TO Stock;

ALTER TABLE books
RENAME COLUMN Published TO Published_Year;


CREATE TABLE cusotmer_id (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(15),
    city VARCHAR(100),
    country VARCHAR(100)
);
alter table cusotmer_id rename to customers;
select* from customers;

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers (customer_id),
    book_id INT REFERENCES books (books_id),
    order_date DATE,
    quantity INT,
    total_amount DECIMAL(10 , 2 )
);

select * from books;
select * from customers;
select * from orders;


-- 1. Retrieve all books from fiction genre.
SELECT 
    *
FROM
    books
WHERE
    genre = 'fiction';
    
-- 2. Find books published after the year 1950
SELECT 
    *
FROM
    books
WHERE
    Published_Year >1950;
    
    
    
-- 3. List Customers from Canada

SELECT 
    *
FROM
    customers
WHERE
    country="canada";
    
    
    
-- 4. Orders Placed in november 2023.
SELECT 
    *
FROM
    orders
WHERE order_date between "2023-11-01" and "2023-11-30";



-- 5. total books available
SELECT 
    SUM(stock) AS total_count
FROM
    books;
    
    

-- 6. find details of most expensive books
SELECT 
    *
FROM
    books
ORDER BY price DESC
LIMIT 1;



-- 7. Show all customers who ordered more than 1 quantity of books
SELECT 
    *
FROM
    customers c
        JOIN
    orders o ON c.customer_id = o.customer_id
WHERE
    o.quantity > 1;
    
-- 8. retrive all orders where total amunt exceeds $20

SELECT 
    * from
      orders where total_amount>20;
      
-- 9. List all genre available int he book table
SELECT 
    distinct genre from books;
    
-- 10. find the book with the lowest stock

SELECT 
    *
FROM
    books order by stock asc limit 1;
    
-- 11.Calculate the total revenue generate from the tables
select * from orders;

select sum(total_amount) as total_rev from orders;

-- 12. Retrive the total number of books sold for each genre
SELECT 
    b.genre, sum(o.quantity) as total_books
FROM
    books b
        JOIN
    orders o ON b.book_id = o.book_id group by genre;

-- 13. Find the average price of books in the fantasy genre.
select avg(price) as average_price from books where genre="fantasy";

-- 14. List the customers who have placed atleast 2 orders
SELECT 
    c.name, COUNT(o.order_id) AS order_count
FROM
    customers c
        JOIN
    orders o ON c.customer_id = o.customer_id
GROUP BY c.name
HAVING COUNT(o.order_id) >= 2;

 
-- 15. find the most frequently ordered book
SELECT 
    b.title, COUNT(o.order_id)
FROM
    books b
        JOIN
    orders o ON b.book_id = o.book_id
GROUP BY b.title
ORDER BY  COUNT(o.order_id) DESC limit 1;

SELECT
    b.title,
    COUNT(o.order_id) AS total_orders
FROM books b
JOIN orders o
ON b.book_id = o.book_id
GROUP BY b.title
ORDER BY total_orders DESC
LIMIT 1;

-- 16 Show the top 3 most expensive books of "fantasy" genre :
SELECT 
    b.title, b.price
FROM
    books b
WHERE
    genre = 'fantasy'
ORDER BY price DESC
LIMIT 3;



-- 17 Retricve the Total Quantity of books sold by each author
SELECT 
    b.author, SUM(o.quantity) total_quantity
FROM
    books b
        JOIN
    orders o ON b.book_id = o.book_id
GROUP BY b.author
ORDER BY total_quantity DESC;


-- 18 list the cities where customers who spent over $30 are located:
SELECT 
    c.city, o.total_amount
FROM
    customers c
        JOIN
    orders o ON c.customer_id = o.customer_id
WHERE
    total_amount > 30
ORDER BY total_amount DESC;


-- 19.find the customer who spend more on orders :
SELECT 
    c.customer_id, c.name, SUM(o.total_amount) AS Total_spent
FROM
    customers c
        JOIN
    orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id
ORDER BY total_spent DESC limit 1;



-- 20 Calculate the stock remaining after fulfillment of all the orders :
SELECT 
    b.book_id,
    b.title,
    b.stock,
    COALESCE(SUM(o.quantity), 0) AS total_quantity,  b.stock-COALESCE(SUM(o.quantity), 0) as remaining_quantity
FROM
    books b
        LEFT JOIN
    orders o ON b.book_id = o.book_id
GROUP BY b.book_id;













  

 




