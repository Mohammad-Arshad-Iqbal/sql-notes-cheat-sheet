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
