## Library Management System Database

This project involves the creation of a SQL database for a library management system, including essential tables to manage branches, employees, members, books, and book issuing and return processes. Below is an overview of the database schema, instructions for creating the database, and the relationships between the tables.

### Database Creation

To create the database and the associated tables, follow these steps:

**Create the Database**
```sql
CREATE DATABASE library_management;
```
### Table Creation
## 1. Branch Table
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

## 2. Employees Table
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

## 3. Members Table
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

## 4. Books Table
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

## 5. Issued Status Table
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

## 6. Return Status Table
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

The database design ensures efficient management of books and tracking of issues and returns, facilitating seamless library operations.
