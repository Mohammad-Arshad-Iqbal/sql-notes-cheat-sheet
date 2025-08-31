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
