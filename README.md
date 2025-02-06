# Customers-Books-Data-Analysis

create database Customersdata;
use Customersdata;

Select * from books;
Select * from customers;
Select * from orders;

-- Questions

-- 1) Retrieve all books in the "Fiction" genre:

select * from books
where genre = 'Fiction';


-- 2) Find books published after the year 1950:

select * from books
where published_year > 1950;

-- 3) List all customers from the Canada:

select * from customers
where country = 'Canada';

-- 4) Show orders placed in November 2023:

SELECT * FROM orders
WHERE order_date BETWEEN '01-11-2023' AND '30-11-2023';


-- 5) Retrieve the total stock of books available:

select sum(stock) AS Total_Stock
from books;


-- 6) Find the details of the most expensive book:

select * from books
order by price desc
limit 1;

-- 7) Show all customers who ordered more than 1 quantity of a book:

select * from orders
where quantity>1;

-- 8) Retrieve all orders where the total amount exceeds $20:

select * from orders
where total_amount>20
order by total_amount;

-- 9) List all genres available in the Books table:

select distinct genre from books; 


-- 10) Find the book with the lowest stock:

select * from books
order by stock asc
limit 1;

-- 11) Calculate the total revenue generated from all orders:

select sum(price) as Total_Revenue
from books;

-- Advance Questions : 

-- 1) Retrieve the total number of books sold for each genre:
-- Join
select b.genre, sum(o.quantity) as Total_Book_Sold
from orders o
join books b on o.book_id = b.book_id
group by b.genre;

-- 2) Find the average price of books in the "Fantasy" genre:

select avg(price) AS AVG_Price_of_Books
from books
where genre = 'Fantasy';


-- 3) List customers who have placed at least 2 orders:

select customer_id, count(order_id) AS Order_Count
from orders
group by customer_id
having count(order_id) >=2
order by customer_id;

-- 4) Find the most frequently ordered book:

SELECT o.book_id, b.title, COUNT(o.book_id) AS Book_Count
FROM orders o
JOIN books b ON o.book_id = b.book_id
GROUP BY o.book_id, b.title
ORDER BY Book_Count DESC
LIMIT 1;

-- 5) Show the top 3 most expensive books of 'Fantasy' Genre :

select * from books
where genre = 'Fantasy'
order by price desc limit 3;


-- 6) Retrieve the total quantity of books sold by each author:

select b.author, sum(o.quantity) AS Total_Book_Qty
from orders o
join books b on o.book_id = b.book_id
group by b.author;

-- 7) List the cities where customers who spent over $30 are located:

select distinct c.city, total_amount
from orders o
join customers c on o.customer_id = c.customer_id
where o.total_amount > 30;

-- 8) Find the customer who spent the most on orders:

select c.customer_id, c.name, sum(o.total_Amount) AS Total_Spent
from Orders o
Join Customers c On o.customer_id = c.customer_id
group by c.customer_id, c.name
order by Total_Spent Desc
limit 1;

-- 9) Calculate the stock remaining after fulfilling all orders;

SELECT b.book_id, b.title, b.stock, COALESCE(SUM(o.quantity), 0) AS Order_Quantity, 
    (b.stock - COALESCE(SUM(o.quantity), 0)) AS Remaining_Quantity
FROM books b
LEFT JOIN orders o ON b.book_id = o.book_id
GROUP BY b.book_id, b.title, b.stock;

