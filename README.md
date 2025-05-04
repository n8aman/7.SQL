# 7.SQL basics
show databases;
use<>;
drop databass
create table<>(id int primary key auto_increment, name varchar(50) not null default 'no name provided', age int not null default 99, created_at TIMESTAMP default CURRENT_TIMESTAMP, updated_at TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,   phone VARCHAR(15) NOT NULL UNIQUE, age INT CHECK (age > 0), word VARCHAR(100) CHECK(REVERSE(word) = word) , CONSTRAINT age_not_negative CHECK (age >= 0),  CONSTRAINT word_is_palindrome CHECK(REVERSE(word) = word),    CONSTRAINT name_address UNIQUE (name , address), 
show tables;
show columns from <tableName>;    desc<tableName>;
drop table <>;
insert into <>(name, age) values(aman, 24);
select * from <> or ;

update <> set age=14 
delete from <> or ;

WHERE 
age=4, name='egg', id=age, name='misty', shirt_size='m', LIKE '%_\%_%', rating IS NULL;
!= 
NOT LIKE '%_____%';
> < >= <= 500; (pages,released_year)
AND OR BETWEEN

SELECT COUNT(*)
select concat(), concat_ws(), substr(title or 'helloworld',1,4 or 7 or -3), replace('helloworld','hell','xyz'), reverse('helloworld'), ucase/lcase(), trim() right/left(), 
select distinct
order by author_lname, 
limit 5, 50;


select max()/min(), sum(), avg(), 
ALTER TABLE <> ADD COLUMN phone VARCHAR(15); ==ALTER TABLE <> ADD COLUMN employee_count INT NOT NULL DEFAULT 1==ALTER TABLE companies DROP COLUMN phone;
RENAME TABLE companies to suppliers; ALTER TABLE suppliers RENAME TO companies; ALTER TABLE companies RENAME COLUMN name TO company_name;
ALTER TABLE companies MODIFY company_name VARCHAR(100) DEFAULT 'unknown';
ALTER TABLE suppliers CHANGE business biz_name VARCHAR(50);
ALTER TABLE houses ADD CONSTRAINT positive_pprice CHECK (purchase_price >= 0);ALTER TABLE houses DROP CONSTRAINT positive_pprice;
PRIMARY KEY, FOREIGN KEY

### date
select curtime(), curdate(), now(),
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


### VIEW
-- INSTEAD OF TYPING THIS QUERY ALL THE TIME...
SELECT title, released_year, genre, rating, first_name, last_name FROM reviews
JOIN series ON series.id = reviews.series_id
JOIN reviewers ON reviewers.id = reviews.reviewer_id;
 -- WE CAN CREATE A VIEW:
CREATE VIEW full_reviews ASSELECT title, released_year, genre, rating, first_name, last_name FROM reviews
JOIN series ON series.id = reviews.series_id
OIN reviewers ON reviewers.id = reviews.reviewer_id;
 
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


### joins
-- To perform a (kind of useless) cross join:SELECT * FROM customers, orders;
-- The order doesn't matter here:SELECT * FROM ordersJOIN customers ON customers.id = orders.customer_id; or vice versa.
JOIN____ON_________
INNER JOIN, LEFT JOIN, RIGHT JOIN,
GROUP BY first_name , last_name
ORDER BY total{first_name, last_name, SUM(amount) AS total};


### GROUP BY
(HAVING, ROLLUP,) COUNT(rating) > 1; GROUP BY title WITH ROLLUP;


### To View Modes:
SELECT @@GLOBAL.sql_mode;
SELECT @@SESSION.sql_mode;
 
-- To Set Them:
SET GLOBAL sql_mode = 'modes';
SET SESSION sql_mode = 'modes';
STRICT_TRANS_TABLE
ONLY_FULL_GROUP_BY
NO_ZERO_IN_DATE


### windowsFunction Using Over()
PARTITION BY, RANK(),  ROW_NUMBER(),  DENSE_RANK(), NTILE(),  FIRST_

### sub- queries
SELECT * FROM orders WHERE customer_id = (SELECT id FROM customers WHERE last_name = 'abc');

### CASE STATEMENTS

============================================================================================

### ER(entity relationship) diagram
Entities – Real-world objects or concepts (e.g., Student, Course).

Attributes – Properties of those entities (e.g., Student Name, Course Title).🔸 Simple Attribute: Cannot be divided further.Example: FirstName 🔸 Composite Attribute: Can be broken into sub-parts.Example: FullName → FirstName + LastName 🔸 Derived Attribute: Can be derived from other attributes.Example: Age from DOB🔸 Key Attribute: Uniquely identifies an entity.Example: StudentID

Relationships – Associations between entities (e.g., A Student enrolls in a Course).🔸 One-to-One (1:1): One entity relates to only one of another.🔸 One-to-Many (1:N): One entity relates to many of another.🔸 Many-to-Many (M:N): Many entities relate to many others.


# SQL Advance.
-- 1) adjusting max packet size to allow large files to run
SET GLOBAL max_allowed_packet = 1073741824;
-- 2) adjusting your SQL mode to allow invalid dates and use a smarter GROUP BY setting
SET GLOBAL SQL_MODE='TRADITIONAL,ALLOW_INVALID_DATES,ONLY_FULL_GROUP_BY';
-- 3) adjusting your timeout settings to run longer queries
SET GLOBAL connect_timeout=28800;
SET GLOBAL wait_timeout=28800;
SET GLOBAL interactive_timeout=28800;
-----------------------------------WORKBENCHSETTINGS--------------------------------------





   
