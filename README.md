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
   - [Members Data](#members-data)
   - [Branch Data](#branch-data)
   - [Employees Data](#employees-data)
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
### Branch Table
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

### Employees Table
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

### Members Table
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

### Books Table
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

### Issued Status Table
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

### Return Status Table
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
