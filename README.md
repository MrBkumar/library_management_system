## Table of Contents
1. [Introduction](#introduction)
2. [Database Structure](#database-structure)
   - [Branch Table](#branch-table)
   - [Employees Table](#employees-table)
   - [Members Table](#members-table)
   - [Books Table](#books-table)
   - [Issued Status Table](#issued-status-table)
   - [Return Status Table](#return-status-table)
3. [Sample Data Insertion](#sample-data-insertion)
   - [Branch Data](#branch-data)
   - [Employees Data](#employees-data)
   - [Members Data](#members-data)
   - [Books Data](#books-data)
   - [Issued Status Data](#issued-status-data)
   - [Return Status Data](#return-status-data)
4. [SQL Queries](#sql-queries)

---
## Introduction
## Library Management System Database

This project involves the creation of a SQL database for a library management system, including essential tables to manage branches, employees, members, books, and book issuing and return processes. Below is an overview of the database schema, instructions for creating the database, and the relationships between the tables.

### Database Structure

To create the database and the associated tables, follow these steps:

**Create the Database**
```sql
CREATE DATABASE library_management;
```

### Table Creation
### Branch Table
The `branch` table stores information about different library branches, including the branch ID, manager, address, and contact number.

- `branch_id`: Unique identifier for each branch.
- `manager_id`: Manager of the branch.
- `branch_address`: Address of the branch.
- `contact_no`: Branch contact number.
```sql
DROP TABLE IF EXISTS branch;
CREATE TABLE branch
(
            branch_id VARCHAR(10) PRIMARY KEY,
            manager_id VARCHAR(10),
            branch_address VARCHAR(30),
            contact_no VARCHAR(15)
);
```

### Employees Table
The `employees` table holds employee information such as employee ID, name, position, salary, and the branch where the employee works.

- `emp_id`: Unique identifier for each employee.
- `emp_name`: Name of the employee.
- `position`: Employee’s position in the library.
- `salary`: Salary of the employee.
- `branch_id`: Foreign key linking to the `branch` table.
```sql
DROP TABLE IF EXISTS employees;
CREATE TABLE employees
(
            emp_id VARCHAR(10) PRIMARY KEY,
            emp_name VARCHAR(30),
            position VARCHAR(30),
            salary DECIMAL(10,2),
            branch_id VARCHAR(10),
            FOREIGN KEY (branch_id) REFERENCES  branch(branch_id)
);
```

### Members Table
The `members` table contains information about library members, including their ID, name, address, and registration date.

- `member_id`: Unique identifier for each member.
- `member_name`: Name of the library member.
- `member_address`: Address of the member.
- `reg_date`: Date of registration.
```sql
DROP TABLE IF EXISTS members;
CREATE TABLE members
(
            member_id VARCHAR(10) PRIMARY KEY,
            member_name VARCHAR(30),
            member_address VARCHAR(30),
            reg_date DATE
);
```

### Books Table
The `books` table stores information about books available in the library, including ISBN, title, category, rental price, status, author, and publisher.

- `isbn`: Unique identifier for each book.
- `book_title`: Title of the book.
- `category`: Book category/genre.
- `rental_price`: Rental price of the book.
- `status`: Availability status of the book (e.g., Available, Rented).
- `author`: Author of the book.
- `publisher`: Publisher of the book.
```sql
DROP TABLE IF EXISTS books;
CREATE TABLE books
(
            isbn VARCHAR(50) PRIMARY KEY,
            book_title VARCHAR(80),
            category VARCHAR(30),
            rental_price DECIMAL(10,2),
            status VARCHAR(10),
            author VARCHAR(30),
            publisher VARCHAR(30)
);
```

### Issued Status Table
The `issued_status` table tracks the books issued to members. It stores the issued ID, member ID, book name, issue date, book ISBN, and the employee who processed the issue.

- `issued_id`: Unique identifier for each issued record.
- `issued_member_id`: Foreign key linking to the `members` table.
- `issued_book_name`: Name of the issued book.
- `issued_date`: Date the book was issued.
- `issued_book_isbn`: Foreign key linking to the `books` table.
- `issued_emp_id`: Foreign key linking to the `employees` table.
```sql
DROP TABLE IF EXISTS issued_status;
CREATE TABLE issued_status
(
            issued_id VARCHAR(10) PRIMARY KEY,
            issued_member_id VARCHAR(30),
            issued_book_name VARCHAR(80),
            issued_date DATE,
            issued_book_isbn VARCHAR(50),
            issued_emp_id VARCHAR(10),
            FOREIGN KEY (issued_member_id) REFERENCES members(member_id),
            FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
            FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn) 
);
```

### Return Status Table
The `return_status` table records the details of returned books, including return ID, issued ID, return book name, return date, and book ISBN.

- `return_id`: Unique identifier for each return record.
- `issued_id`: Foreign key linking to the `issued_status` table.
- `return_book_name`: Name of the returned book.
- `return_date`: Date the book was returned.
- `return_book_isbn`: Foreign key linking to the `books` table.
```sql
DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
(
            return_id VARCHAR(10) PRIMARY KEY,
            issued_id VARCHAR(30),
            return_book_name VARCHAR(80),
            return_date DATE,
            return_book_isbn VARCHAR(50),
            FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);
```

###Extra
```sql
-- Adding new column in return_status

ALTER TABLE return_status
ADD Column book_quality VARCHAR(15) DEFAULT('Good');

UPDATE return_status
SET book_quality = 'Damaged'
WHERE issued_id 
    IN ('IS112', 'IS117', 'IS118');
SELECT * FROM return_status;
```

The database design ensures efficient management of books and tracking of issues and returns, facilitating seamless library operations.

### Sample Data Insertion
### Branch Data
```sql
-- Insert values into branch table
INSERT INTO branch(branch_id, manager_id, branch_address, contact_no) 
VALUES
('B001', 'E109', '100 Main St', '+14028887676'),
('B002', 'E109', '200 Elm Rd', '+14028887677'),
('B003', 'E109', '300 Cedar Ave', '+14028887678'),
('B004', 'E110', '400 Pine Blvd', '+14028887679'),
('B005', 'E110', '500 Oakwood St', '+14028887680');
SELECT * FROM branch;
```

### Employees Data
```sql
-- Insert values into employees table
INSERT INTO employees(emp_id, emp_name, position, salary, branch_id) 
VALUES
('E101', 'John Doe', 'Clerk', 60000.00, 'B001'),
('E102', 'Jane Taylor', 'Clerk', 45000.00, 'B002'),
('E103', 'Michael Harris', 'Librarian', 55000.00, 'B001'),
('E104', 'Emily Johnson', 'Assistant', 40000.00, 'B001'),
('E105', 'Sarah Parker', 'Assistant', 42000.00, 'B001'),
('E106', 'Linda Evans', 'Assistant', 43000.00, 'B001'),
('E107', 'David Walker', 'Clerk', 62000.00, 'B005'),
('E108', 'Laura Hall', 'Clerk', 46000.00, 'B004'),
('E109', 'Daniel King', 'Manager', 57000.00, 'B003'),
('E110', 'Anna Brooks', 'Manager', 41000.00, 'B005'),
('E111', 'Christopher Wright', 'Assistant', 65000.00, 'B005');
SELECT * FROM employees;
```

### Members Data
```sql
-- Insert values into members table
INSERT INTO members(member_id, member_name, member_address, reg_date) 
VALUES
('C101', 'Alice Johnson', '123 Maple Dr', '2021-05-15'),
('C102', 'Bob Anderson', '456 Oak Ln', '2021-06-20'),
('C103', 'Carol Smith', '789 Cedar Ave', '2021-07-10'),
('C104', 'David Lee', '101 Pine St', '2021-08-05'),
('C105', 'Evelyn Davis', '234 Birch Blvd', '2021-09-25'),
('C106', 'Frank Turner', '345 Spruce Ct', '2021-10-15'),
('C107', 'Grace Martin', '567 Walnut St', '2021-11-20'),
('C108', 'Henry Thomas', '890 Elm St', '2021-12-10'),
('C109', 'Ivy Peterson', '901 Oakwood Dr', '2022-01-05'),
('C110', 'Jack Miller', '234 Redwood Rd', '2022-02-25'),
('C118', 'Sophia Wright', '350 Cherry St', '2024-06-01'),    
('C119', 'James Wilson', '400 Pinecrest Ave', '2024-05-01');
SELECT * FROM members;
```

### Books Data
```sql
-- Inserting into books table 
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher) 
VALUES
('978-0-7432-7356-4', 'The Hobbit', 'Fantasy', 7.00, 'yes', 'J.R.R. Tolkien', 'Houghton Mifflin Harcourt'),
('978-0-393-05081-8', 'A People\'s History of the United States', 'History', 9.00, 'yes', 'Howard Zinn', 'Harper Perennial'),
('978-0-7432-4722-4', 'The Da Vinci Code', 'Mystery', 8.00, 'yes', 'Dan Brown', 'Doubleday'),
('978-0-307-58837-1', 'Sapiens: A Brief History of Humankind', 'History', 8.00, 'no', 'Yuval Noah Harari', 'Harper Perennial'),
('978-0-14-143951-8', 'Pride and Prejudice', 'Classic', 5.00, 'yes', 'Jane Austen', 'Penguin Classics'),
('978-0-451-52993-5', 'Fahrenheit 451', 'Dystopian', 5.50, 'yes', 'Ray Bradbury', 'Ballantine Books'),
('978-0-06-025492-6', 'Where the Wild Things Are', 'Children', 3.50, 'yes', 'Maurice Sendak', 'HarperCollins'),
('978-0-7432-4722-5', 'Angels & Demons', 'Mystery', 7.50, 'yes', 'Dan Brown', 'Doubleday'),
('978-0-679-76489-8', 'Harry Potter and the Sorcerer\'s Stone', 'Fantasy', 7.00, 'yes', 'J.K. Rowling', 'Scholastic'),
('978-0-330-25864-8', 'Animal Farm', 'Classic', 5.50, 'yes', 'George Orwell', 'Penguin Books');
SELECT * FROM books;
```

### Issued Status Data
```sql
-- Inserting into issued_status table
INSERT INTO issued_status(issued_id, issued_member_id, issued_book_name, issued_date, issued_book_isbn, issued_emp_id) 
VALUES
('IS106', 'C106', 'Animal Farm', '2024-03-10', '978-0-330-25864-8', 'E104'),
('IS107', 'C107', 'The Da Vinci Code', '2024-03-11', '978-0-7432-4722-4', 'E104'),
('IS108', 'C108', 'Fahrenheit 451', '2024-03-12', '978-0-451-52993-5', 'E104'),
('IS109', 'C109', 'Sapiens: A Brief History of Humankind', '2024-03-13', '978-0-307-58837-1', 'E105'),
('IS110', 'C110', 'Pride and Prejudice', '2024-03-14', '978-0-14-143951-8', 'E105');
SELECT * FROM issued_status;
```

### Return Status Data
```sql
-- Inserting into return_status table
INSERT INTO return_status(return_id, issued_id, return_date) 
VALUES
('RS101', 'IS106', '2024-04-10'),
('RS102', 'IS107', '2024-04-12'),
('RS103', 'IS108', '2024-04-14'),
('RS104', 'IS109', '2024-04-16'),
('RS105', 'IS110', '2024-04-18');
SELECT * FROM return_status;
```

### SQL Queries
### 1. List all employees working in the library.
```sql
SELECT * FROM employees;
```

### 2. Find all members who registered after January 1, 2023.
```sql
SELECT * FROM members
WHERE reg_date > '2021-01-01';
```

### 3. Display the titles of all books along with their rental price.
```sql
SELECT
book_title, rental_price
FROM books;
```

### 4. Find all branches that have the word "Main" in their address.
```sql
SELECT
*
FROM branch
WHERE branch_address ILIKE '%Main%';
```

### 5. Get the contact number of a branch where the branch ID is 'B001'.
```sql
SELECT
branch_id, contact_no
FROM branch
WHERE branch_id = 'B001';
```

### 6. List the names of all employees along with their branch address.
```sql
SELECT e.emp_name, b.branch_address 
FROM employees e 
JOIN branch b ON e.branch_id = b.branch_id;
```

### 7. Get the total number of books issued by each employee.
```sql
SELECT
issued_emp_id,
COUNT(*) as total_issued
FROM issued_status
GROUP BY 1;
```

### 8. List all employees whose salary is greater than 3000.
```sql
SELECT
*
FROM employees
WHERE salary > 45000;
```

### 8. List all employees whose salary is greater than average salary.
```sql
SELECT
*
FROM booksemployees
WHERE salary > ROUND((
	SELECT
	AVG(salary)
	FROM employees), 2);
```

### 9. Find the total number of books in each category.
```sql
SELECT
category, COUNT(*) AS total_books
FROM books
GROUP BY 1
ORDER BY 2 DESC;
```

### 10. Get the details of the employee who issued a book with ISBN '978-0-525-47535-5'.
```sql
SELECT
e.*
FROM issued_status i
INNER JOIN employees e ON  i.issued_emp_id = e.emp_id
WHERE i.issued_book_isbn = '978-0-525-47535-5';
```

### 11. Find all books that are not currently available (status = 'issued').
```sql
SELECT
*
FROM books
WHERE status ILIKE '%No%';
```

### 12. Find all members who have borrowed books by author "J.K. Rowling".
```sql
SELECT
m.member_name
FROM issued_status i
INNER JOIN members m ON m.member_id = i.issued_member_id
INNER JOIN books b ON i.issued_book_isbn = b.isbn
WHERE b.author ILIKE '%J.K. Rowling%';
```

### 13. List all books along with their issue and return status.
```sql
SELECT
b.book_title, b.status, i.issued_date, r.return_date
FROM books b
INNER JOIN issued_status i ON b.isbn = i.issued_book_isbn
INNER JOIN return_status r ON i.issued_id = r.issued_id;
```

### 14. Find the employees who have issued more than 5 books.
```sql
SELECT
issued_emp_id, COUNT(*) AS issued_count
FROM issued_status
GROUP BY 1
HAVING COUNT(*) > 5
```

### 15. Find the employee details who have issued more than 5 books.
```sql
SELECT
*
FROM employees
WHERE emp_id IN (SELECT
issued_emp_id
FROM issued_status
GROUP BY 1
HAVING COUNT(*) > 5);
```

### 16. List the top 3 employees who have issued the most books.
```sql
SELECT
issued_emp_id, COUNT(*) AS issued_count
FROM issued_status
GROUP BY 1
ORDER BY 2 DESC
LIMIT 3;
```

### 17. Get the member name, book title, and return date for all returned books.
```sql
SELECT
m.member_name, b.book_title, r.return_date
FROM members m
INNER JOIN issued_status i ON m.member_id = i.issued_member_id
INNER JOIN books b ON i.issued_book_isbn = b.isbn
INNER JOIN return_status r ON i.issued_id = r.issued_id;
```

### 18. Display all employees who work at branches managed by an employee with ID 'E001'.
```sql
SELECT
*
FROM employees e
INNER JOIN branch b ON e.branch_id = b.branch_id
WHERE b.manager_id ILIKE '%E109%';
```

### 19. Find all employees who work at a branch that has issued at least 10 books.
```sql
SELECT
e.emp_name
FROM employees e
INNER JOIN issued_status i ON e.emp_id = i.issued_emp_id
GROUP BY e.emp_name, e.branch_id
HAVING COUNT(i.issued_book_isbn) >= 5;
```

### 20. Find the total rental price of all books issued by each branch
```sql
SELECT
br.branch_id, SUM(bo.rental_price) AS total_rental_price
FROM branch br
INNER JOIN employees e ON br.branch_id = e.branch_id
INNER JOIN issued_status i ON i.issued_emp_id = e.emp_id
INNER JOIN books bo ON i.issued_book_isbn = bo.isbn
GROUP BY br.branch_id;
```

### 21. Get the branch name and the number of employees working at each branch.
```sql
SELECT
br.branch_address, COUNT(e.emp_id) AS no_emp_working
FROM branch br
INNER JOIN employees e ON e.branch_id = br.branch_id
GROUP BY 1
ORDER BY 2 DESC;
```

### 22. List all books that have never been issued.
```sql
SELECT
*
FROM books bo
WHERE NOT EXISTS (
SELECT 1 FROM issued_status i WHERE i.issued_book_isbn = bo.isbn
)
```

### 23. Find the top 3 branches with the most books issued, but only consider books issued in the last 6 months. List the branch details, total books issued, and the names of the top issuing employees at each branch.
```sql
WITH RecentIssues AS (
	SELECT
	i.issued_emp_id, br.branch_id, COUNT(i.issued_id) AS total_issued_books
	FROM issued_status i
	JOIN employees e ON i.issued_emp_id = e.emp_id
	JOIN branch br ON e.branch_id = br.branch_id
	WHERE i.issued_date >= CURRENT_DATE - INTERVAL '6 months'
	GROUP BY i.issued_emp_id, br.branch_id
)
SELECT
br.branch_address, e.emp_name, ri.total_issued_books
FROM branch br
JOIN RecentIssues ri ON br.branch_id = ri.branch_id
JOIN employees e ON ri.issued_emp_id = e.emp_id
ORDER BY ri.total_issued_books
LIMIT 3;
```

### 24. Find the member who has rented the most expensive books (by total rental cost) in the last year. Display their total rental expenditure, number of books rented, and the details of the most expensive book they rented.
```sql
WITH MemberExpenditure AS (
    SELECT m.member_id, m.member_name, SUM(b.rental_price) AS total_spent, COUNT(b.isbn) AS total_books, MAX(b.rental_price) AS max_price
    FROM members m
    JOIN issued_status i ON m.member_id = i.issued_member_id
    JOIN books b ON i.issued_book_isbn = b.isbn
    WHERE i.issued_date >= CURRENT_DATE - INTERVAL '1 year'
    GROUP BY m.member_id, m.member_name
)
SELECT me.member_id, me.member_name, me.total_spent, me.total_books, b.book_title, b.rental_price
FROM MemberExpenditure me
JOIN books b ON b.rental_price = me.max_price
ORDER BY me.total_spent DESC
LIMIT 1;
```

### 25. List all books that have been issued more than twice but have never been returned.
```sql
SELECT
b.book_title, b.isbn, COUNT(i.issued_id) AS issue_count
FROM books b
JOIN issued_status i ON b.isbn = i.issued_book_isbn
LEFT JOIN return_status r ON i.issued_id = r.issued_id
WHERE r.issued_id IS NULL
GROUP BY 1, 2
HAVING COUNT(i.issued_id) > 2;
```

### 26.Find the employee who has issued the highest number of unique books across multiple branches and calculate the total number of unique books issued.
```sql
WITH EmployeeBookCount AS (
	SELECT
	issued_emp_id, COUNT(DISTINCT issued_book_isbn) AS unique_book
	FROM issued_status
	GROUP BY 1
)
SELECT
e.emp_name, ebc.unique_book
FROM EmployeeBookCount ebc
JOIN employees e ON e.emp_id = ebc.issued_emp_id
ORDER BY 2 DESC
LIMIT 1;
```

### 27. Find the total revenue generated by all book rentals, but only include books that were rented and returned within the same month.
```sql
SELECT SUM(b.rental_price) AS total_revenue
FROM books b
JOIN issued_status i ON b.isbn = i.issued_book_isbn
JOIN return_status r ON i.issued_id = r.issued_id
WHERE DATE_PART('month', i.issued_date) = DATE_PART('month', r.return_date)
AND DATE_PART('year', i.issued_date) = DATE_PART('year', r.return_date);
```

### 28. Calculate the total revenue generated by each employee from books they issued, and compare it to the average revenue generated by employees across all branches. List only employees whose revenue exceeds the average.
```sql
WITH EmployeeRevenue AS (
    SELECT i.issued_emp_id, SUM(b.rental_price) AS total_revenue
    FROM issued_status i
    JOIN books b ON i.issued_book_isbn = b.isbn
    GROUP BY i.issued_emp_id
),
GlobalAvgRevenue AS (
    SELECT AVG(er.total_revenue) AS avg_revenue
    FROM EmployeeRevenue er
)
SELECT e.emp_name, er.total_revenue
FROM EmployeeRevenue er
JOIN employees e ON er.issued_emp_id = e.emp_id
CROSS JOIN GlobalAvgRevenue gar
WHERE er.total_revenue > gar.avg_revenue;
```

### 29. Get a list of members who have never issued any books.
```sql
SELECT
    m.member_id,
    m.member_name
FROM
    members m
LEFT JOIN
    issued_status i ON m.member_id = i.issued_member_id
WHERE
    i.issued_id IS NULL;
```

### 30. Update the salary of all employees who have issued more than 5 books, giving them a 5% raise.
```sql
UPDATE employees
SET salary = salary * 0.95
WHERE emp_id IN (
    SELECT issued_emp_id
    FROM issued_status
    GROUP BY issued_emp_id
    HAVING COUNT(issued_id) > 5
);
SELECT * FROM EMPLOYEES;
```
