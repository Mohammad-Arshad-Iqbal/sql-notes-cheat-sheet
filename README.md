## Sql-notes-cheat-sheet


## 1. SELECT Statement  

The **SELECT** statement is used to retrieve data from a table. You can fetch all columns, specific columns, or even create calculated/custom columns for better readability.  

### **Retrieve all columns**  
Fetch all records and all columns from the `Employees` table.  

```sql
SELECT * FROM Employees;
```

### **Retrieve specific column**

Fetch only the FirstName column from the Employees table.
```sql
SELECT FirstName FROM Employees;
```
### **Retrieve multiple specific columns**  

Fetch the FirstName and Salary columns from the `Employees` table.  

```sql
SELECT FirstName, Salary FROM Employees;
```
### **Concatenate First and Last Name**  

Combine `FirstName` and `LastName` into one column without alias.  

```sql
SELECT EmployeeID, CONCAT(FirstName, ' ', LastName) FROM Employees;
```
### **Concatenate with custom column name (without AS)**  

Assign a custom column name using square brackets (SQL Server syntax).  

```sql
SELECT EmployeeID, CONCAT(FirstName, ' ', LastName) [Full Name] FROM Employees;
```
### **Concatenate with alias using AS**  

Rename the concatenated column using the **AS** keyword for better clarity.  

```sql
SELECT EmployeeID, CONCAT(FirstName, ' ', LastName) AS [Full Name] FROM Employees;
```
## 2. DISTINCT Keyword  

The **DISTINCT** keyword is used to return only unique (non-duplicate) values from a column or a combination of columns. It helps to avoid repeating data in your query results.  
### **Distinct First Name**  

Fetch unique values of the `FirstName` column.  

```sql
SELECT DISTINCT FirstName FROM EmployeeRecords;
```

### **Distinct First Name and Last Name**  

Fetch unique combinations of `FirstName` and `LastName`.  

```sql
SELECT DISTINCT FirstName, LastName FROM EmployeeRecords;
```
### **Distinct Salary**  

Fetch unique salary values from the table.  

```sql
SELECT DISTINCT Salary FROM EmployeeRecords;
```
### **Distinct across all columns**  

Fetch only unique rows across all columns in the table.  

```sql
SELECT DISTINCT * FROM EmployeeRecords;
```
## 3. Temporary Tables  

In SQL Server, **temporary tables** are used to store intermediate results. They are created in the `tempdb` database and can be either **local** (only visible in the current session) or **global** (accessible across multiple sessions).  

- **Local Temporary Table (`#temp`)**: Accessible only within the current session.  
- **Global Temporary Table (`##temp`)**: Accessible to all sessions until the last session using it is closed.  

---

### **Create a Local Temporary Table from Employees**  
Copy all data from the `Employees` table into a local temporary table `#temp1`.  It will access in within that query file where it is created

```sql
SELECT * INTO #temp1
FROM [dbo].[Employees];
```
### **Create a Global Temporary Table from EmployeeRecords**  

Copy all data from the `EmployeeRecords` table into a global temporary table `##temp2`.  

```sql
SELECT * INTO ##temp2
FROM [dbo].[EmployeeRecords];
```
Local Temporary Table (#): A table that exists only in your current session and is automatically deleted when the session ends. Only you can see and use it.

Global Temporary Table (##): A table that can be accessed by all sessions and stays until all sessions using it are closed. Everyone can see and use it while it exists.
## 4. WHERE Clause  

The **WHERE** clause is used to filter records based on a condition. It allows you to retrieve only those rows that meet specific criteria, such as matching values, ranges, or logical expressions.  

---

### **Filter by EmployeeID**  
Fetch all columns where `EmployeeID` equals 2.  

```sql
SELECT * 
FROM [dbo].[EmployeeRecords]
WHERE EmployeeID = 2;
```
### **Filter by Salary greater than or equal to 75,000**  

Fetch all records where the `Salary` is greater than or equal to 75000.00.  

```sql
SELECT * 
FROM dbo.EmployeeRecords 
WHERE Salary >= 75000.00;
```
### **Distinct values with condition**  

Fetch unique combinations of `FirstName`, `LastName`, `Department`, and `Salary` where `Salary` is less than 75000.00.  

```sql
SELECT DISTINCT FirstName, LastName, Department, Salary
FROM dbo.EmployeeRecords 
WHERE Salary < 75000.00;
```
## 5. ORDER BY Clause  

The **ORDER BY** clause is used to sort the result set in either **ascending (ASC)** or **descending (DESC)** order. By default, the sorting is ascending if not specified. You can sort by one or multiple columns.  

---

### **Sort by Salary (Ascending by default)**  
Fetch all records and sort them by `Salary` in ascending order.  

```sql
SELECT * 
FROM [dbo].[Employees]
ORDER BY Salary;
```
### **Sort by Salary (Descending)**  

Fetch all records and sort them by `Salary` in descending order.  

```sql
SELECT * 
FROM [dbo].[Employees]
ORDER BY Salary DESC;
```
### **Sort by First Name (Ascending) and Salary (Descending)**  

First sort by `FirstName` in ascending order. If multiple employees share the same `FirstName`, then sort them by `Salary` in descending order.  

```sql
SELECT * 
FROM dbo.Employees
ORDER BY FirstName ASC, Salary DESC;
```
### **Sort by Department (Ascending) and Salary (Descending)**  

First sort employees by `Department` in ascending order. Within each department, sort them by `Salary` in descending order.  

```sql
SELECT * 
FROM dbo.Employees
ORDER BY Department ASC, Salary DESC;
```
## 6. AND / OR Operators  

The **AND** and **OR** operators are used in the **WHERE** clause to combine multiple conditions.  

- **AND**: Returns rows only if **all conditions are TRUE**.  
- **OR**: Returns rows if **at least one condition is TRUE**.  
- You can also group conditions using parentheses `()` for better control of logical evaluation.  

---

### **AND Example (Match Two Conditions)**  
Fetch all records where `LastName` is `'Miller'` **and** `EmployeeID` equals `3`.  

```sql
SELECT * 
FROM EmployeeRecords
WHERE LastName = 'Miller' AND EmployeeID = 3;
```
### **AND with Different Data Type Check**  

Here `EmployeeID` is compared as a string (less common, but still valid in SQL Server).  

```sql
SELECT * 
FROM EmployeeRecords
WHERE LastName = 'Miller' AND EmployeeID = '3';
```
### **OR Example (Either Condition True)**  

Fetch all records where `Department` is `'HR'` or `'Finance'`.  

```sql
SELECT * 
FROM dbo.EmployeeRecords
WHERE Department = 'HR' OR Department = 'Finance';
```
### **Combining AND and OR with Parentheses**  

Fetch employees where `Department` is either `'HR'` or `'Finance'`, and their `Salary` equals 60000.  

```sql
SELECT * 
FROM dbo.EmployeeRecords
WHERE (Department = 'HR' OR Department = 'Finance') AND Salary = 60000;
```
## 7. BETWEEN, IN, and NOT Operators  

SQL provides several operators to make filtering more flexible:  

- **BETWEEN**: Selects values within a given range (inclusive).  
- **IN**: Allows you to specify multiple values in the `WHERE` clause (shorter than multiple ORs).  
- **NOT**: Excludes rows that match a condition.  

---

### **Retrieve all records**  
Fetch all rows from the `EmployeeRecords` table.  

```sql
SELECT * 
FROM dbo.EmployeeRecords;
```
### **NOT Operator (Exclude conditions)**  

Fetch all records where `FirstName` is not `'John'` and `Salary` is not 60000.  

```sql
SELECT * 
FROM dbo.EmployeeRecords
WHERE NOT FirstName = 'John' AND NOT Salary = 60000;
```
### **BETWEEN Operator (Range filter)**  

Fetch employees where `Salary` is between 75,000 and 85,000.  

```sql
SELECT * 
FROM dbo.EmployeeRecords
WHERE Salary BETWEEN 75000 AND 85000;
```
here it will also rows where salary is exactly 75000 and 85000 mean include lower and upper bound
### **NOT BETWEEN Operator (Exclude range)**  

Fetch employees where `Salary` is not between 75,000 and 85,000.  

```sql
SELECT * 
FROM dbo.EmployeeRecords
WHERE Salary NOT BETWEEN 75000 AND 85000;
```
### **IN Operator (Multiple values)**  

Fetch employees where `Department` is either `'HR'` or `'IT'`.  

```sql
SELECT * 
FROM dbo.EmployeeRecords
WHERE Department IN ('HR', 'IT');
```
### **NOT IN Operator (Exclude multiple values)**  

Fetch employees where `Department` is not `'HR'` or `'IT'`.  

```sql
SELECT * 
FROM dbo.EmployeeRecords
WHERE Department NOT IN ('HR', 'IT');
```
## 8. INSERT INTO Statement  

The **INSERT INTO** statement is used to add new rows of data into a table. You can either:  
1. Specify column names (recommended).  
2. Insert values into all columns without specifying column names (order must match table structure).  

---

### **View existing data in Employees table**  
Fetch all current records from the `Employees` table.  

```sql
SELECT * 
FROM dbo.Employees;
```
### **Insert a complete row (all columns specified)**  

Add a new employee record with all details provided.  

```sql
INSERT INTO dbo.Employees (EmployeeID, FirstName, LastName, Department, Salary, HireDate)
VALUES (6, 'Raj', 'Ambani', 'IT', 67000, '2023-04-20');
```
### **Insert partial data (only selected columns)**  

Add a new employee by specifying only `EmployeeID`, `FirstName`, and `LastName`.  
Other columns will take default values or remain `NULL`.  

```sql
INSERT INTO dbo.Employees (EmployeeID, FirstName, LastName)
VALUES (7, 'Rohit', 'Mehera');
```
### **Insert all columns (without specifying names)**  

Add a new employee by providing values for all columns in order.  

```sql
INSERT INTO dbo.Employees
VALUES (8, 'Mahesh', 'Narang', 'HR', 73000, '2024-01-21');
```
Note: This requires that you provide values for every column in the exact order they are defined in the table.
### **Check table structure (metadata)**  

View column names, data types, and constraints of the `Employees` table using the system view `INFORMATION_SCHEMA.COLUMNS`.  

```sql
SELECT * 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'Employees';
```
## 9. SQL NULL OPERATOR  

The `NULL` operator is used to **check for missing or unknown values** in a database.  

---

### 1. Insert Records with NULL or Empty Values  

```sql
INSERT INTO dbo.Employees
VALUES (9,'Jay','','IT',73000,'2022-04-04');

INSERT INTO dbo.Employees
VALUES (10,'Nitin','shamani','0',54000,'2021-02-22');
```
📝 **These two statements insert new records:**  

- In the first, `LastName` is given as an empty string `''`.  
- In the second, `Department` is given as `'0'` (string, not `NULL`).  

⚡ **Learning Note:**  
- An empty string (`''`) is **not** the same as `NULL`.  
- `'0'` is just text, not a missing value.  
### **Incorrect NULL Comparison**  

```sql
SELECT * 
FROM dbo.Employees 
WHERE Department = NULL;
```
❌ This will not return any rows.  

**Why?**  
Because `NULL` means *"unknown"*, and SQL cannot compare unknown values using `=`.
### **Correct NULL Check**  

```sql
SELECT * 
FROM dbo.Employees 
WHERE Department IS NULL;
```
✅ Returns rows where the `Department` column has `NULL` values.
### **Check for NOT NULL**  

```sql
SELECT * 
FROM dbo.Employees 
WHERE Department IS NOT NULL;
```
✅ Returns rows where the `Department` column has valid values (not `NULL`).
## 10. SQL UPDATE STATEMENT  

The `UPDATE` statement is used to **modify existing records** in a table.  

---

### 1. Update Department Where Value is NULL  

```sql
UPDATE #1
SET Department = 'HR'
WHERE Department IS NULL;
```
✅ Updates all rows in table #1 where `Department` is `NULL` and sets it to `'HR'`.  

⚡ **Learning Note:** Always use a `WHERE` clause to avoid updating all rows unintentionally.  
### **Update Multiple Columns for a Specific Employee**  

```sql
UPDATE #1
SET Salary = 89000, HireDate = '2023-01-01'
WHERE EmployeeID = 7;
```
✅ Updates the `Salary` and `HireDate` for the employee whose `EmployeeID = 7`.  
### **Update All Rows in a Table**  

```sql
UPDATE #2
SET Department = 'Finance';
```
✅ Updates the `Department` column for all rows in table #2, setting the value to `'Finance'`.  

⚠️ **Warning:** Without a `WHERE` clause, this affects every row in the table.
## 11.SQL DELETE, TRUNCATE, and DROP Statements  

These commands are used to **remove records or tables** in SQL.  

---

### **Delete Specific Records**  

```sql
DELETE FROM #3
WHERE LastName = '' OR Department = '0';
```
✅ Deletes rows in #3 where `LastName` is empty (`''`) or `Department` equals `'0'`.  

⚡ **Learning Note:** `DELETE` removes only the rows matching the `WHERE` condition.
### **Delete All Records from a Table**  

```sql
DELETE FROM #4;
```
✅ Deletes all rows from table #4, but keeps the table structure intact.  

⚠️ **Be careful:** Running `DELETE` without a `WHERE` removes every record.
### **Truncate Table**  

```sql
TRUNCATE TABLE #3;
```
✅ Removes all rows from table #3 but keeps the structure intact.  

⚡ **Note:** Faster than `DELETE` because it does not log individual row deletions.  

❌ Cannot use `WHERE` with `TRUNCATE`.
### **Drop Table**  

```sql
DROP TABLE #3;
```
✅ Deletes the entire table #3, including its structure.
### **Difference Between DELETE, TRUNCATE, and DROP**  

- **DELETE** → Removes specific rows (or all if no `WHERE`). Table structure remains.  
- **TRUNCATE** → Removes all rows quickly. Table structure remains.  
- **DROP** → Removes all rows and deletes the table structure itself.  

## 12.SQL COMMENTS and TOP Clause  

---

### 1. Single-Line Comment  

```sql
-- Hi we are learning SQL Server
```
### **Multi-Line Comment**  

```sql
/*
Hi
we
are
learning
SQL
server
*/
SELECT * FROM dbo.Employees;
```
### **Using SELECT TOP**  

```sql
SELECT TOP 2 * FROM dbo.Employees;
```
🔎 Returns the first 2 rows from the `Employees` table.  
`TOP n` is used to limit the number of rows returned.  

⚡ Commonly used with `ORDER BY` to get the "Top N" highest/lowest records.
## 13.SQL MIN, MAX & GROUP BY  

The `MIN()` and `MAX()` aggregate functions return the **smallest** and **largest** values in a column.  
`GROUP BY` is used to organize rows into groups for aggregation.  

---

### 1. Maximum Total Amount  

```sql
SELECT MAX(TotalAmount) [Maximum Amount] 
FROM dbo.Sales;
```
### 2. Maximum Quantity Sold for Each ProductID  

```sql
-- Maximum Quantity sold for each productID
SELECT ProductID, MAX(Quantity) [Maximum Quantity] 
FROM dbo.Sales
GROUP BY ProductID;
```
✅ Groups sales by `ProductID` and returns the maximum quantity sold for each product.
### **Minimum Total Amount for Each StoreID**  

```sql
-- Show minimum TotalAmount for each StoreID
SELECT StoreID, MIN(TotalAmount) [Minimum Total Amount] 
FROM dbo.Sales
GROUP BY StoreID;
```
✅ Groups sales by `StoreID` and shows the minimum `TotalAmount` for each store.
## 14.SQL SUM, AVG, COUNT & GROUP BY  

The aggregate functions `SUM()`, `AVG()`, and `COUNT()` are used to perform calculations on numeric or categorical data.  
`GROUP BY` helps organize rows into groups for aggregation.  

---

### 1. SUM – Total Quantity  

```sql
SELECT SUM(Quantity) [Total Quantity] 
FROM dbo.Sales;
```
✅ Returns the sum of all quantities sold.
### **AVG – Average Quantity**  

```sql
SELECT AVG(Quantity) [Average Quantity] 
FROM dbo.Sales;
```
✅ Returns the average quantity sold across all records.
### **SUM & AVG for Each Product**  

```sql
-- Sum of Quantity, sum of TotalAmount, avg of Quantity, avg of TotalAmount for each distinct product
SELECT 
    ProductID,
    SUM(Quantity) AS [Total Quantity],
    SUM(TotalAmount) AS [Sum of Amount],
    AVG(Quantity)   AS [Average Quantity Sold],
    AVG(TotalAmount) AS [Average Amount]
FROM dbo.Sales
GROUP BY ProductID;
```
### **COUNT – Number of Rows in table**  

```sql
SELECT COUNT(*) [Number of Rows] 
FROM dbo.Sales;
```
✅ Counts all rows in the Sales table.
### **COUNT on a Column**  

```sql
SELECT COUNT(PaymentMethod) [No of Records] 
FROM dbo.Sales;
```
✅ Counts all non-NULL values in the PaymentMethod column.
### **COUNT DISTINCT – Unique Products**  

```sql
SELECT COUNT(DISTINCT ProductID) [Distinct Products] 
FROM dbo.Sales;
```
### **Count of Records by Payment Method**  

```sql
SELECT PaymentMethod, COUNT(*) [Pay Mode] 
FROM dbo.Sales
GROUP BY PaymentMethod;
```
✅ Groups sales by PaymentMethod and counts the number of records for each payment mode.
- **COUNT(*)** → Counts all rows (including `NULL`s).  
- **COUNT(column)** → Counts only non-`NULL` values in that column.  
- **COUNT(DISTINCT column)** → Counts distinct values in that column.
## 15.SQL GROUP BY & HAVING Clause  

The `GROUP BY` clause groups rows that have the same values into summary rows, often used with aggregate functions (`SUM`, `AVG`, `COUNT`, etc.).  
The `HAVING` clause is like `WHERE`, but it filters results **after aggregation**.It is used for selection of group mean which group we need based on condition for example we only need those group where number of element is 10 or sum within group is 1000.

---

### Example – GROUP BY with HAVING  

```sql
-- Total Sales, Avg Sales, Total Quantity, Avg Quantity for each distinct product
SELECT 
    ProductID,
    SUM(TotalAmount) [Sum of Sales],
    SUM(Quantity)    [Total Quantity],
    AVG(TotalAmount) [Avg Amount],
    AVG(Quantity)    [Avg Quantity]
FROM dbo.Sales
GROUP BY ProductID
HAVING SUM(TotalAmount) < 700 AND SUM(Quantity) = 21;
```
## Difference Between WHERE and HAVING  

| Feature              | WHERE Clause 🟦                          | HAVING Clause 🟥                          |
|-----------------------|-------------------------------------------|-------------------------------------------|
| **Usage**            | Filters rows **before aggregation**       | Filters groups **after aggregation**       |
| **Works With**       | Raw data rows                            | Aggregated results (`SUM`, `AVG`, etc.)    |
| **Aggregate Functions** | Cannot be used with aggregate functions directly (except in subqueries) | Can be used with aggregate functions       |
| **Execution Order**  | Applied first                            | Applied after `GROUP BY`                   |

---


## 16.SQL INNER JOIN  

The `INNER JOIN` keyword selects records that have **matching values** in both tables.  

---

### Example Tables  

#### Table: Employees  

| EmployeeID | FirstName | LastName | DepartmentID |
|------------|-----------|----------|--------------|
| 1          | John      | Doe      | 101          |
| 2          | Jane      | Smith    | 102          |
| 3          | Alice     | Johnson  | 103          |
| 4          | Bob       | Brown    | 101          |
| 5          | Emily     | Davis    | 104          |

#### Table: Departments  

| DepartmentID | DepartmentName |
|--------------|----------------|
| 101          | HR             |
| 102          | IT             |
| 103          | Finance        |
| 105          | Marketing      |

---

### INNER JOIN Query  

```sql
SELECT 
    e.EmployeeID,
    e.FirstName,
    e.LastName,
    d.DepartmentName
FROM Employees e
INNER JOIN Departments d
    ON e.DepartmentID = d.DepartmentID;
```
#### Table: Output
| EmployeeID | FirstName | LastName | DepartmentName |
|------------|-----------|----------|----------------|
| 1          | John      | Doe      | HR             |
| 2          | Jane      | Smith    | IT             |
| 3          | Alice     | Johnson  | Finance        |
| 4          | Bob       | Brown    | HR             |

- The INNER JOIN returns only the rows where DepartmentID exists in both Employees and Departments tables.

- Employee Emily (DepartmentID = 104) does not appear in the result because DepartmentID 104 does not exist in Departments table.
## 17.SQL LEFT JOIN  

The `LEFT JOIN` keyword returns **all records from the left table (Employees)**, and the **matched records from the right table (Departments)**.  
If there is no match, NULL values are returned for columns from the right table.  

---

### Example Tables  

#### Table: Employees  

| EmployeeID | FirstName | LastName | DepartmentID |
|------------|-----------|----------|--------------|
| 1          | John      | Doe      | 101          |
| 2          | Jane      | Smith    | 102          |
| 3          | Alice     | Johnson  | 103          |
| 4          | Bob       | Brown    | 101          |
| 5          | Emily     | Davis    | 104          |

#### Table: Departments  

| DepartmentID | DepartmentName |
|--------------|----------------|
| 101          | HR             |
| 102          | IT             |
| 103          | Finance        |
| 105          | Marketing      |

---

### LEFT JOIN Query  

```sql
SELECT 
    e.EmployeeID,
    e.FirstName,
    e.LastName,
    d.DepartmentName
FROM Employees e
LEFT JOIN Departments d
    ON e.DepartmentID = d.DepartmentID;
```
#### Table: Output  
| EmployeeID | FirstName | LastName | DepartmentName |
|------------|-----------|----------|----------------|
| 1          | John      | Doe      | HR             |
| 2          | Jane      | Smith    | IT             |
| 3          | Alice     | Johnson  | Finance        |
| 4          | Bob       | Brown    | HR             |
| 5          | Emily     | Davis    | NULL           |
- All employees are shown (from the left table).

- Emily’s DepartmentID = 104 does not exist in the Departments table, so her DepartmentName is NULL.

- Department Marketing (105) is not shown because no employee belongs to it (but will appear if we use RIGHT JOIN).
## 18.SQL FULL OUTER JOIN  

The `FULL OUTER JOIN` keyword returns **all records** when there is a match in either the left table (Employees) or the right table (Departments).  
If there is no match, NULL values are returned for the missing side.  

---

### Example Tables  

#### Table: Employees  

| EmployeeID | FirstName | LastName | DepartmentID |
|------------|-----------|----------|--------------|
| 1          | John      | Doe      | 101          |
| 2          | Jane      | Smith    | 102          |
| 3          | Alice     | Johnson  | 103          |
| 4          | Bob       | Brown    | 101          |
| 5          | Emily     | Davis    | 104          |

#### Table: Departments  

| DepartmentID | DepartmentName |
|--------------|----------------|
| 101          | HR             |
| 102          | IT             |
| 103          | Finance        |
| 105          | Marketing      |

---

### FULL OUTER JOIN Query  

```sql
SELECT 
    e.EmployeeID,
    e.FirstName,
    e.LastName,
    d.DepartmentName
FROM Employees e
FULL OUTER JOIN Departments d
    ON e.DepartmentID = d.DepartmentID;
```
#### Output
| EmployeeID | FirstName | LastName | DepartmentName |
|------------|-----------|----------|----------------|
| 1          | John      | Doe      | HR             |
| 2          | Jane      | Smith    | IT             |
| 3          | Alice     | Johnson  | Finance        |
| 4          | Bob       | Brown    | HR             |
| 5          | Emily     | Davis    | NULL           |
| NULL       | NULL      | NULL     | Marketing      |
- Includes all employees (from Employees) and all departments (from Departments).

- Emily’s DepartmentID = 104 has no matching department, so DepartmentName = NULL.

- Department Marketing (105) has no matching employee, so EmployeeID, FirstName, and LastName are NULL.
## SQL ANTI LEFT JOIN  

An **ANTI LEFT JOIN** is not a separate keyword in SQL.  
It is implemented using a `LEFT JOIN` combined with a `WHERE` condition checking for `NULL`.  

It helps to find records in the **left table** that have **no matching record** in the right table.  

---

### Example Tables  

#### Table: Employees  

| EmployeeID | FirstName | LastName | DepartmentID |
|------------|-----------|----------|--------------|
| 1          | John      | Doe      | 101          |
| 2          | Jane      | Smith    | 102          |
| 3          | Alice     | Johnson  | 103          |
| 4          | Bob       | Brown    | 101          |
| 5          | Emily     | Davis    | 104          |

#### Table: Departments  

| DepartmentID | DepartmentName |
|--------------|----------------|
| 101          | HR             |
| 102          | IT             |
| 103          | Finance        |
| 105          | Marketing      |

---

### Anti Left Join Query  

```sql
-- Find employees whose DepartmentID does not exist in Departments table
SELECT 
    e.EmployeeID,
    e.FirstName,
    e.LastName,
    e.DepartmentID
FROM Employees e
LEFT JOIN Departments d
    ON e.DepartmentID = d.DepartmentID
WHERE d.DepartmentID IS NULL;
```
#### Table: Output
| EmployeeID | FirstName | LastName | DepartmentID |
|------------|-----------|----------|--------------|
| 5          | Emily     | Davis    | 104          |
- Employee Emily (DepartmentID = 104) does not have a matching department in the Departments table.

- Hence, only Emily is returned in the result.
## SQL SELF JOIN  

A **SELF JOIN** is a regular join, but the table is joined with itself.  
We use table **aliases** to differentiate between the two instances of the same table.  

It is commonly used to find relationships within a table, like:  
- Employees and their managers  
- Comparing rows in the same dataset  

---

### Example Table: Employees  

| EmployeeID | FirstName | LastName | ManagerID |
|------------|-----------|----------|-----------|
| 1          | John      | Doe      | NULL      |
| 2          | Jane      | Smith    | 1         |
| 3          | Alice     | Johnson  | 1         |
| 4          | Bob       | Brown    | 2         |
| 5          | Emily     | Davis    | 3         |

---

### Self Join Query  

```sql
-- Show employees with their managers
SELECT 
    e.EmployeeID,
    e.FirstName AS EmployeeName,
    m.FirstName AS ManagerName
FROM Employees e
INNER JOIN Employees m
    ON e.ManagerID = m.EmployeeID;
```
### Table: output
| EmployeeID | EmployeeName | ManagerName |
|------------|--------------|-------------|
| 2          | Jane         | John        |
| 3          | Alice        | John        |
| 4          | Bob          | Jane        |
| 5          | Emily        | Alice       |
The table Employees is joined with itself.

- Alias e → represents employees.

- Alias m → represents managers.

- Employees with ManagerID = NULL (like John) do not appear in the result.
