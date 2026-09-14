
# SQL Complete Mastery & Interview Cheatsheet

> **How to use this vault note:** This guide covers SQL from foundational building blocks to advanced interview query patterns. Each section contains syntax, plain-English conceptual breakdowns, sample schemas, and runnable examples.

---

## 1. Database Architecture & Classification

### A. RDBMS Architecture
A **Relational Database Management System (RDBMS)** stores structured data organized into relations (tables) consisting of rows (tuples/records) and columns (attributes/fields). Relationships between entities are enforced via keys.

### B. SQL Command Categories

| Category | Full Name | Purpose | Core Commands |
| :--- | :--- | :--- | :--- |
| **DDL** | Data Definition Language | Defines, alters, or destroys database structure/schema. | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DML** | Data Manipulation Language | Modifies records stored within tables. | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Data Query Language | Retrieves data without modifying state. | `SELECT` |
| **TCL** | Transaction Control Language | Manages unit-of-work state and atomic operations. | `COMMIT`, `ROLLBACK` |
| **DCL** | Data Control Language | Manages access rights and security privileges. | `GRANT`, `REVOKE` |

---

## 2. Constraints & Data Integrity

Constraints enforce business logic and data validity rules directly at the database engine level.

*   **`NOT NULL`**: Ensures a column cannot accept or store `NULL` values.
*   **`UNIQUE`**: Guarantees all values in a column are distinct across the table.
*   **`PRIMARY KEY`**: Combination of `NOT NULL` + `UNIQUE`. Uniquely identifies each record. 
*   **`FOREIGN KEY`**: Enforces referential integrity. Ensures values in a child table match existing primary key values in a parent table.

---

## 3. DDL (Data Definition Language)

### Sample Baseline Schema
```sql
-- Create Parent Table: Departments
CREATE TABLE Departments (
    DepartmentID INT NOT NULL AUTO_INCREMENT,
    DepartmentName VARCHAR(100) NOT NULL,
    Location VARCHAR(100) DEFAULT 'Gurugram',
    CONSTRAINT PK_Departments PRIMARY KEY (DepartmentID)
);

-- Create Child Table: Employees
CREATE TABLE Employees (
    EmployeeID INT NOT NULL AUTO_INCREMENT,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    Salary DECIMAL(10, 2) CHECK (Salary > 0),
    HireDate DATE NOT NULL,
    DepartmentID INT,
    CONSTRAINT PK_Employees PRIMARY KEY (EmployeeID),
    CONSTRAINT FK_Department FOREIGN KEY (DepartmentID) 
        REFERENCES Departments(DepartmentID)
);
````

### Destruction Commands: `DROP` vs `TRUNCATE` vs `DELETE`

|**Command**|**Category**|**Removes Structure?**|**Where Clause?**|**Rollback Possible?**|**Speed**|
|---|---|---|---|---|---|
|**`DELETE`**|DML|No (Removes rows one-by-one)|Yes (`WHERE`)|Yes|Slow|
|**`TRUNCATE`**|DDL|No (Deallocates entire data pages)|No|Database dependent|Fast|
|**`DROP`**|DDL|Yes (Deletes data and table schema)|No|Generally No|Instant|

## 4. DML (Data Manipulation Language)

### Insert Records

SQL

```
INSERT INTO Departments (DepartmentName, Location)
VALUES 
    ('Engineering', 'Gurugram'),
    ('Quality Assurance', 'Noida');

INSERT INTO Employees (FirstName, LastName, Email, Salary, HireDate, DepartmentID)
VALUES 
    ('Akshay', 'Sharma', 'akshay@farelabs.com', 45000.00, '2026-09-15', 1),
    ('Pooja', 'Verma', 'pooja.v@farelabs.com', 52000.00, '2025-03-10', 1);
```

### Update Records

SQL

```
UPDATE Employees
SET Salary = Salary * 1.10
WHERE DepartmentID = 1 AND HireDate < '2026-01-01';
```

### Delete Records

SQL

```
DELETE FROM Employees
WHERE EmployeeID = 4;
```

## 5. Aggregations, GROUP BY, and HAVING

Aggregate functions process multiple column values to return a single computed metric (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`).

  

### `WHERE` vs `HAVING`

- `WHERE`: Filters individual rows **before** grouping and aggregations occur. Cannot accept aggregate functions.
    
      
    
- `HAVING`: Filters aggregated summary records **after** the `GROUP BY` execution.
    
      
    

SQL

```
-- Find departments with more than 1 employee and average salary > 40000
SELECT 
    DepartmentID, 
    COUNT(EmployeeID) AS Headcount,
    AVG(Salary) AS AverageSalary
FROM Employees
WHERE DepartmentID IS NOT NULL
GROUP BY DepartmentID
HAVING COUNT(EmployeeID) > 1 AND AVG(Salary) > 40000.00;
```

## 6. Comprehensive SQL Joins

### A. INNER JOIN

Retrieves records when there is a match in **both** tables.

  

SQL

```
SELECT e.FirstName, d.DepartmentName
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

### B. LEFT (OUTER) JOIN

Retrieves **all** records from the left table (`Employees`), along with matching records from the right table (`Departments`). Unmatched right-table columns output `NULL`.

  

SQL

```
SELECT e.FirstName, d.DepartmentName
FROM Employees e
LEFT JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

### C. RIGHT (OUTER) JOIN

Retrieves **all** records from the right table, along with matched records from the left table.

  

SQL

```
SELECT e.FirstName, d.DepartmentName
FROM Employees e
RIGHT JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

## 7. Database Normalization Rules

Normalization systematically decomposes tables to eliminate data redundancy and anomalies (Insertion, Deletion, Update anomalies).

  

|**Normal Form**|**Rule Requirement**|**Example Violation & Resolution**|
|---|---|---|
|**1NF** (First)|Atomic values per cell. Each record has a unique identifier.|_Violation:_ Column `Skill = 'Java, SQL'`<br><br>  <br>  <br><br>_Fix:_ Separate rows for each individual skill.|
|**2NF** (Second)|Must satisfy 1NF. No partial dependencies.|_Violation:_ Table with composite PK `(OrderID, ProductID)` having `CustomerAddress`.<br><br>  <br>  <br><br>_Fix:_ Move customer data to `Orders` table.|
|**3NF** (Third)|Must satisfy 2NF. No transitive dependencies.|_Violation:_ Table has `StudentID`, `ZipCode`, and `City`.<br><br>  <br>  <br><br>_Fix:_ Move `ZipCode` and `City` to a `Locations` table.|

## 8. Transactions & ACID Mechanics

A transaction is a logical unit of database processing containing one or more SQL statements executed atomically.

  

SQL

```
START TRANSACTION;

UPDATE Accounts SET Balance = Balance - 5000.00 WHERE AccountID = 101;
UPDATE Accounts SET Balance = Balance + 5000.00 WHERE AccountID = 102;

COMMIT; -- Or ROLLBACK if an error occurs
```

- **Atomicity**: Both balance updates occur, or neither occurs.
    
      
    
- **Consistency**: Account balances cannot drop below configured `CHECK` limits.
    
      
    
- **Isolation**: Concurrently executing transactions cannot see uncommitted changes.
    
      
    
- **Durability**: Once `COMMIT` executes, disk writes are persisted even during total system power loss.
    

## 9. Top Interview SQL Query Patterns

### Pattern 1: Find the N-th Highest Salary

SQL

```
-- Method 1: Using subqueries (Finds 2nd Highest)
SELECT MAX(Salary) AS SecondHighestSalary
FROM Employees
WHERE Salary < (SELECT MAX(Salary) FROM Employees);

-- Method 2: Using LIMIT/OFFSET (MySQL / PostgreSQL)
-- For 3rd highest salary (OFFSET = N - 1):
SELECT DISTINCT Salary
FROM Employees
ORDER BY Salary DESC
LIMIT 1 OFFSET 2;
```

### Pattern 2: Identify Duplicate Records

SQL

```
-- Identify duplicate emails and their frequency
SELECT Email, COUNT(*) AS OccurrenceCount
FROM Employees
GROUP BY Email
HAVING COUNT(*) > 1;
```

### Pattern 3: Delete Duplicate Records (Retaining One)

SQL

```
-- Deleting duplicate rows while keeping the record with the lowest EmployeeID
DELETE e1 FROM Employees e1
INNER JOIN Employees e2 
ON e1.Email = e2.Email AND e1.EmployeeID > e2.EmployeeID;
```