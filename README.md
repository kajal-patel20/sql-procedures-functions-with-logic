# sql-procedures-functions-with-logic

# SQL Procedures & Functions with Parameters and Conditional Logic

This project demonstrates the use of **stored procedures** and **user-defined functions** in SQL with a focus on:

- Creating and using `CREATE PROCEDURE` and `CREATE FUNCTION`
- Implementing **parameters** and **conditional logic**


### ✅ Features Covered

1. **Stored Procedure with Parameters**  
   - Handles conditional logic (e.g., insert/update based on input)
2. **Scalar Function with Logic**  
   - Returns a computed result using input parameters
3. **Demonstrates control flow using `IF`, `ELSE`, and `CASE`**


## 🧱 Database Structure

- `Employees`: Stores employee info (ID, name, salary, department)
- `Departments`: Stores department details

# 💻 SQL Script

Creating the data
```
-- Create Departments table
CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(100)
);

-- Create Employees table
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(100),
    Salary DECIMAL(10,2),
    DepartmentID INT,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);
```

Inserting the data
```
-- Insert sample departments
INSERT INTO Departments VALUES 
(1, 'Engineering'),
(2, 'Sales'),
(3, 'HR');

-- Insert some initial employees
INSERT INTO Employees VALUES
(101, 'Kajal', 85000, 1),
(102, 'Naved', 45000, 2),
(103, 'Zanib', 30000, 3);
```

## 1. Procedure
```
CREATE PROCEDURE ManageEmployee
    @EmployeeID INT,
    @Name VARCHAR(100),
    @Salary DECIMAL(10,2),
    @DepartmentID INT
AS
BEGIN
    IF EXISTS (SELECT 1 FROM Employees WHERE EmployeeID = @EmployeeID)
    BEGIN
        UPDATE Employees
        SET Name = @Name, Salary = @Salary, DepartmentID = @DepartmentID
        WHERE EmployeeID = @EmployeeID;
    END
    ELSE
    BEGIN
        INSERT INTO Employees (EmployeeID, Name, Salary, DepartmentID)
        VALUES (@EmployeeID, @Name, @Salary, @DepartmentID);
    END
END;
```
## 2. Function
```
CREATE FUNCTION GetTaxCategory (
    @Salary DECIMAL(10,2)
)
RETURNS VARCHAR(20)
AS
BEGIN
    DECLARE @TaxBracket VARCHAR(20);

    SET @TaxBracket = CASE
        WHEN @Salary < 40000 THEN 'Low'
        WHEN @Salary BETWEEN 40000 AND 80000 THEN 'Medium'
        ELSE 'High'
    END;

    RETURN @TaxBracket;
END;
```

## 3. Call Scalar Function
```
SELECT dbo.GetTaxCategory(85000) AS TaxBracket;
```

   
