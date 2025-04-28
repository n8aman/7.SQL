# 7.SQL
show databases;
use<>;
drop database
create table<>(id int primary key auto_increment, name varchar(50) not null default 'no name provided', age int not null default 99, created_at TIMESTAMP default CURRENT_TIMESTAMP, updated_at TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,   phone VARCHAR(15) NOT NULL UNIQUE, age INT CHECK (age > 0), word VARCHAR(100) CHECK(REVERSE(word) = word) , CONSTRAINT age_not_negative CHECK (age >= 0),  CONSTRAINT word_is_palindrome CHECK(REVERSE(word) = word),    CONSTRAINT name_address UNIQUE (name , address), 
show tables;
show columns from <tableName>;    desc<tableName>;
drop table <>;
insert into <>(name, age) values(aman, 24);
select * from <> or ;
update <> set age=14 
delete from <> or ;


WHERE age=4, name='egg', id=age, name='misty', shirt_size='m', LIKE '%_\%_%', rating IS NULL;
!= 
NOT LIKE '%_____%';
> < >= <= 500; (pages,released_year)
AND OR BETWEEN

select concat(), concat_ws(), substr(title or 'helloworld',1,4 or 7 or -3), replace('helloworld','hell','xyz'), reverse('helloworld'), ucase/lcase(), trim() right/left(), 
select distinct
order by author_lname, 
limit 5, 50;
SELECT COUNT(*)
# GROUP BY---------------->(HAVING, ROLLUP,) COUNT(rating) > 1; GROUP BY title WITH ROLLUP;
select max()/min(), sum(), avg(), 
select curtime(), curdate(), now(),
ALTER TABLE <> ADD COLUMN phone VARCHAR(15); ==ALTER TABLE <> ADD COLUMN employee_count INT NOT NULL DEFAULT 1==ALTER TABLE companies DROP COLUMN phone;
RENAME TABLE companies to suppliers; ALTER TABLE suppliers RENAME TO companies; ALTER TABLE companies RENAME COLUMN name TO company_name;
ALTER TABLE companies MODIFY company_name VARCHAR(100) DEFAULT 'unknown';
ALTER TABLE suppliers CHANGE business biz_name VARCHAR(50);
ALTER TABLE houses ADD CONSTRAINT positive_pprice CHECK (purchase_price >= 0);ALTER TABLE houses DROP CONSTRAINT positive_pprice;
PRIMARY KEY, FOREIGN KEY
INNER JOIN, LEFT JOIN, RIGHT JOIN, 

What's a good use case for CHAR? -- Used for text that we know has a fixed length, e.g., State abbreviations, -- abbreviated company names, etc.

What's the difference between DATETIME and TIMESTAMP?-- They both store datetime information, but there's a difference in the range, -- TIMESTAMP has a smaller range. TIMESTAMP also takes up less space.-- TIMESTAMP is used for things like meta-data about when something is created-- or updated.
SELECT CURTIME();
SELECT CURDATE();
SELECT DAYOFWEEK(CURDATE());
SELECT DAYOFWEEK(NOW());
SELECT DATE_FORMAT(NOW(), '%w') + 1;
SELECT DAYNAME(NOW());
SELECT DATE_FORMAT(NOW(), '%W');
SELECT DATE_FORMAT(CURDATE(), '%m/%d/%Y');
SELECT DATE_FORMAT(NOW(), '%M %D at %k:%i');

SELECT * FROM people WHERE birthtime BETWEEN CAST('12:00:00' AS TIME) AND CAST('16:00:00' AS TIME);
SELECT * FROM people WHERE HOUR(birthtime)BETWEEN 12 AND 16;


# VIEW
-- INSTEAD OF TYPING THIS QUERY ALL THE TIME...
SELECT 
    title, released_year, genre, rating, first_name, last_name
FROM
    reviews
        JOIN
    series ON series.id = reviews.series_id
        JOIN
    reviewers ON reviewers.id = reviews.reviewer_id;
 
-- WE CAN CREATE A VIEW:
CREATE VIEW full_reviews AS
SELECT title, released_year, genre, rating, first_name, last_name FROM reviews
JOIN series ON series.id = reviews.series_id
JOIN reviewers ON reviewers.id = reviews.reviewer_id;
 
-- NOW WE CAN TREAT THAT VIEW AS A VIRTUAL TABLE 
-- (AT LEAST WHEN IT COMES TO SELECTING)
SELECT * FROM full_reviews;
CREATE VIEW ordered_series AS
SELECT * FROM series ORDER BY released_year;
 
CREATE OR REPLACE VIEW ordered_series AS
SELECT * FROM series ORDER BY released_year DESC;
 
ALTER VIEW ordered_series AS
SELECT * FROM series ORDER BY released_year;
DROP VIEW ordered_series;

# joins
-- To perform a (kind of useless) cross join:SELECT * FROM customers, orders;
-- The order doesn't matter here:SELECT * FROM ordersJOIN customers ON customers.id = orders.customer_id; or vice versa.
JOIN____ON_________
INNER JOIN, LEFT JOIN, RIGHT JOIN, 
GROUP BY first_name , last_name
ORDER BY total{first_name, last_name, SUM(amount) AS total};




# -- To View Modes:
SELECT @@GLOBAL.sql_mode;
SELECT @@SESSION.sql_mode;
 
-- To Set Them:
SET GLOBAL sql_mode = 'modes';
SET SESSION sql_mode = 'modes';
STRICT_TRANS_TABLE
ONLY_FULL_GROUP_BY
NO_ZERO_IN_DATE



# windowsFunction Using Over() PARTITION BY, RANK(),  ROW_NUMBER(),  DENSE_RANK(), NTILE(),  FIRST_


# CASE STATEMENTS
SELECT title, released_year,
CASE
	WHEN released_year >= 2000 THEN 'modern lit'
    ELSE '20th century lit' 
END AS genre
FROM books;
 
 
SELECT 
    title,
    stock_quantity,
    CASE
        WHEN stock_quantity BETWEEN 0 AND 40 THEN '*'
        WHEN stock_quantity BETWEEN 41 AND 70 THEN '**'
        WHEN stock_quantity BETWEEN 71 AND 100 THEN '***'
        WHEN stock_quantity BETWEEN 101 AND 140 THEN '****'
        ELSE '*****'
    END AS stock
FROM
    books;
 
 
SELECT 
    title,
    stock_quantity,
    CASE
        WHEN stock_quantity <= 40 THEN '*'
        WHEN stock_quantity <= 70 THEN '**'
        WHEN stock_quantity <= 100 THEN '***'
        WHEN stock_quantity <= 140 THEN '****'
        ELSE '*****'
    END AS stock
FROM
    books;
-----------------------------------------
CASE
        WHEN COUNT(*) = 1 THEN '1 book'
        ELSE CONCAT(COUNT(*), ' books')
	END AS count
-----------------------------------------
CASE
    WHEN title LIKE '%stories%' THEN 'Short Stories'
    WHEN title = 'Just Kids' THEN 'Memoir' 
    WHEN title = 'A Heartbreaking Work of Staggering Genius' THEN 'Memior'
    ELSE 'Novel'

END AS type
----------------------------------------
-- USING CASE 
SELECT 
    first_name,
    last_name,
    COUNT(rating) AS count,
    IFNULL(MIN(rating), 0) AS min,
    IFNULL(MAX(rating), 0) AS max,
    ROUND(IFNULL(AVG(rating), 0), 2) AS average,
    CASE
        WHEN COUNT(rating) >= 10 THEN 'POWERUSER'
        WHEN COUNT(rating) > 0 THEN 'ACTIVE'
        ELSE 'INACTIVE'
    END AS status
FROM
    reviewers
        LEFT JOIN
    reviews ON reviewers.id = reviews.reviewer_id
GROUP BY first_name , last_name;
---------------------------------
-- USING IF 
SELECT 
    first_name,
    last_name,
    COUNT(rating) AS count,
    IFNULL(MIN(rating), 0) AS min,
    IFNULL(MAX(rating), 0) AS max,
    ROUND(IFNULL(AVG(rating), 0), 2) AS average,
    IF(COUNT(rating) > 0,
        'ACTIVE',
        'INACTIVE') AS status
FROM
    reviewers
        LEFT JOIN
    reviews ON reviewers.id = reviews.reviewer_id
GROUP BY first_name , last_name;
--------------------------------------------------

# sub- queries
SELECT * FROM orders WHERE customer_id = (SELECT id FROM customers WHERE last_name = 'abc');








































CREATE TABLE customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(50)
);
 
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_date DATE,
    amount DECIMAL(8 , 2 ),
    customer_id INT,
    FOREIGN KEY (customer_id)
        REFERENCES customers (id)
        ON DELETE CASCADE
);
   
