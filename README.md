# MS-SQL-Query
Different types of Queries

# Schema
-- Create Department Table
CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(50) NOT NULL
);

-- Create Employees Table
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    DepartmentID INT,
    ManagerID INT,
    HireDate DATE,
    Salary DECIMAL(10, 2),
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);

-- Populate Departments
INSERT INTO Departments VALUES (1, 'Engineering'), (2, 'Product'), (3, 'Marketing'), (4, 'HR');

-- Populate Employees
INSERT INTO Employees VALUES 
(101, 'John', 'Doe', 1, NULL, '2020-01-15', 95000),
(102, 'Jane', 'Smith', 1, 101, '2020-03-10', 85000),
(103, 'Michael', 'Brown', 2, NULL, '2021-05-20', 90000),
(104, 'Emily', 'Davis', 1, 101, '2022-02-01', 75000),
(105, 'Chris', 'Wilson', 3, NULL, '2019-11-12', 70000),
(106, 'Anna', 'Taylor', 2, 103, '2023-01-10', 65000);
INSERT INTO Employees VALUES
(107, 'Tina', 'Macron', 2, 103, '2023-01-10', 65000,'','','',''),
(108, 'Rina', 'Macron', 2, 103, '2023-01-10', 65000,'','','','');

CREATE TABLE Attendance (
    AttendanceID INT PRIMARY KEY IDENTITY(1,1),
    EmployeeID INT,
    AttendanceDate DATE
);

INSERT INTO Attendance (EmployeeID, AttendanceDate) VALUES 
(101, '2026-04-01'), 
(101, '2026-04-02'), 
(101, '2026-04-03'), -- Consecutive
(101, '2026-04-06'), -- Gap on 4th and 5th (Weekend)
(101, '2026-04-07'), -- Consecutive
(102, '2026-04-01'), 
(102, '2026-04-03'); -- Gap on 2nd

CREATE TABLE MonthlyRevenue (
    RevenueID INT PRIMARY KEY IDENTITY(1,1),
    Month DATE,
    TotalRevenue DECIMAL(18, 2)
);

INSERT INTO MonthlyRevenue (Month, TotalRevenue) VALUES 
('2026-01-01', 50000.00),
('2026-02-01', 55000.00), -- 10% Increase
('2026-03-01', 48000.00), -- Decrease
('2026-04-01', 60000.00); -- Significant Increase

CREATE TABLE EmployeeSkills (
    SkillID INT PRIMARY KEY IDENTITY(1,1),
    EmployeeID INT,
    SkillName VARCHAR(50),
    ProficiencyLevel VARCHAR(20), -- e.g., Beginner, Intermediate, Expert
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);

-- Adding skills for the employees we created earlier
INSERT INTO EmployeeSkills (EmployeeID, SkillName, ProficiencyLevel) VALUES 
(101, 'SQL', 'Expert'),
(101, 'C#', 'Expert'),
(101, '.NET', 'Expert'),
(102, 'SQL', 'Intermediate'),
(102, 'C#', 'Beginner'),
(103, 'SQL', 'Expert'),
(103, 'Python', 'Intermediate'),
(104, 'SQL', 'Beginner'),
(105, 'Marketing Strategy', 'Expert');

-- Add the BirthDate column
ALTER TABLE Employees ADD BirthDate DATE;

-- Populate with sample data
UPDATE Employees SET BirthDate = '1990-05-15' WHERE EmployeeID = 101; -- Birthday has passed
UPDATE Employees SET BirthDate = '1985-11-20' WHERE EmployeeID = 102; -- Birthday coming up
UPDATE Employees SET BirthDate = '1995-04-12' WHERE EmployeeID = 103; -- Birthday tomorrow!
UPDATE Employees SET BirthDate = '2000-01-01' WHERE EmployeeID = 104; 
UPDATE Employees SET BirthDate = '1992-08-30' WHERE EmployeeID = 105;

CREATE TABLE Sales (
    SaleID INT PRIMARY KEY IDENTITY(1,1),
    Region VARCHAR(50),
    TotalRevenue DECIMAL(18, 2),
    TotalBookings INT,
    SaleDate DATE
);

INSERT INTO Sales (Region, TotalRevenue, TotalBookings, SaleDate) VALUES 
('Asia', 5000.00, 10, '2026-04-01'),
('Europe', 8000.00, 20, '2026-04-01'),
('USA', 0.00, 0, '2026-04-02'),      -- Potential Divide by Zero
('Asia', 3000.00, 5, '2026-04-02'),
('Europe', 0.00, 0, '2026-04-03'),     -- Potential Divide by Zero
('USA', 4500.00, 12, '2026-04-03');

-- Add the contact columns
ALTER TABLE Employees ADD PersonalEmail VARCHAR(100);
ALTER TABLE Employees ADD WorkEmail VARCHAR(100);
ALTER TABLE Employees ADD MobileNumber VARCHAR(20);

-- Populate with mixed NULL data to test COALESCE
UPDATE Employees SET WorkEmail = 'mit.shah@corp.com', PersonalEmail = 'mit@gmail.com' WHERE EmployeeID = 101;
UPDATE Employees SET WorkEmail = 'jane.d@corp.com', PersonalEmail = NULL WHERE EmployeeID = 102;
UPDATE Employees SET WorkEmail = NULL, PersonalEmail = 'mike.p@yahoo.com' WHERE EmployeeID = 103;
UPDATE Employees SET WorkEmail = NULL, PersonalEmail = NULL, MobileNumber = '9876543210' WHERE EmployeeID = 104;
UPDATE Employees SET WorkEmail = NULL, PersonalEmail = NULL, MobileNumber = NULL WHERE EmployeeID = 105;

CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY IDENTITY(1,1),
    customer_id INT NOT NULL,
    transaction_date DATE NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(20) DEFAULT 'Completed'
);


INSERT INTO transactions (customer_id, transaction_date, amount)
VALUES 
(101, '2025-05-15', 120.00), (101, '2025-06-10', 85.50),
(101, '2025-07-22', 200.00), (101, '2025-08-05', 50.00),
(101, '2025-09-18', 110.00), (101, '2025-10-12', 95.00),
(101, '2025-11-25', 300.00), (101, '2025-12-30', 150.00),
(101, '2026-01-14', 45.00),  (101, '2026-02-20', 210.00),
(101, '2026-03-05', 80.00),  (101, '2026-04-15', 130.00),
(102, '2025-05-20', 400.00), (102, '2025-06-15', 150.00),
(102, '2026-04-01', 500.00);


CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    order_amount DECIMAL(10, 2) NOT NULL
);

INSERT INTO orders (order_id, customer_id, order_date, order_amount)
VALUES 
(1, 101, '2026-01-01', 100.00),
(2, 101, '2026-01-15', 50.00),
(3, 101, '2026-02-10', 200.00),
(4, 102, '2026-01-05', 300.00),
(5, 102, '2026-02-05', 150.00);

# Sargable and non sargable query
--In SQL Server, **SARGable** stands for **S**earch **ARG**umentable. 

--A query is **SARGable** if the SQL Server engine can take advantage of an index to speed up the execution. A query is **non-SARGable** if it's written in a way that forces the engine to ignore the index and scan every single row in the table (a **Table Scan** or **Index Scan**), which is a major performance killer.

-----

--### The Golden Rule
--> **Never wrap your indexed column inside a function in the `WHERE` clause.**

--If you apply a function to the column, SQL Server can't "see" the values inside the index to find the match quickly. It has to calculate the function for every row first.

---

--### Example 1: Dates (The most common mistake)
--Suppose you have an index on the `HireDate` column.

--#### ❌ Non-SARGable (Slow)
--The engine has to extract the year from every row before comparing it.
--```sql
SELECT EmployeeID, FirstName 
FROM Employees 
WHERE YEAR(HireDate) = 2024;
--```

--#### ✅ SARGable (Fast)
--The engine can look at the index and quickly jump to the range of dates starting with 2024.
--```sql
SELECT EmployeeID, FirstName 
FROM Employees 
WHERE HireDate >= '2024-01-01' AND HireDate < '2025-01-01';
```



---

--### Example 2: Strings and Wildcards
--Suppose you have an index on the `LastName` column.

--#### ❌ Non-SARGable (Slow)
--Using a wildcard at the **beginning** of a string prevents the use of an index because the engine doesn't know where to start looking.
--```sql
SELECT * FROM Employees WHERE LastName LIKE '%Shah';
```

--#### ✅ SARGable (Fast)
--A wildcard at the **end** allows the engine to use the index to find all names starting with "Sha".
--```sql
SELECT * FROM Employees WHERE LastName LIKE 'Shah%';
```

---

--### Example 3: String Manipulations
--#### ❌ Non-SARGable (Slow)
--```sql
SELECT * FROM Employees WHERE LEFT(FirstName, 3) = 'Mit';
--```

--#### ✅ SARGable (Fast)
--```sql
SELECT * FROM Employees WHERE FirstName LIKE 'Mit%';
--```

---

--### How to identify this in an interview?

--If you are at Agoda and they show you a slow query, look for these **"SARGable Killers"**:
--1.  **Functions on columns:** `ABS(Column)`, `UPPER(Column)`, `TRIM(Column)`.
--2.  **Arithmetic on columns:** `WHERE Salary * 12 > 100000` (Fix: `WHERE Salary > 100000 / 12`).
--3.  **Leading wildcards:** `LIKE '%text'`.
--4.  **ISNULL wrappers:** `WHERE ISNULL(Status, 0) = 1` (Fix: `WHERE Status = 1 OR Status IS NULL`).



--### Why this matters for a Senior Role:
--As a developer with 9 years of experience, knowing the syntax is expected—but knowing **why** a query is slow is what defines a Senior Engineer. Mentioning that non-SARGable queries cause high **Logical Reads** and **CPU spikes** will show the Agoda interviewers that you care about database health and scalability.

--Would you like to see how to check if a query is SARGable using the **Execution Plan**?

--The "Reverse Index" Optimization
--If you frequently need to search by email domain in a massive table, a common "Senior" trick is to store the reversed email address in a persisted computed column and index that.

--Original: mit.shah@corp.com

--Reversed: moc.proc@hahs.tim

--You would then query for LIKE 'moc.proc@%'. Because the wildcard is now at the end, SQL Server can perform an Index Seek, which is significantly faster.

# Queries
Select * from departments;
select * from employees;
select * from attendance;
select * from monthlyrevenue;
select * from EmployeeSkills;
select * from Sales;
select * from transactions;
select * from orders;

--1. The "Nth" Highest Salary

--Non corelated sub query
select top 1 salary, firstname from
(Select Top 2 salary, firstname from employees order by salary desc)as salaryDetails order by salary asc

--Window function
Select salary,firstname from(
select firstName, 
	   salary,
	   rank() over (order by salary desc) as Rank
	   from
	   Employees) as t where Rank = 2

--2. Department Stats (Aggregations & Joins)
-- Get department names and their average salary, 
-- but only for departments with more than 1 employee.

select d.DepartMentId, Avg(e.Salary) as AvgSalary, STRING_AGG(e.FirstName, ', ') WITHIN GROUP (ORDER BY e.FirstName) AS EmployeeNames
from departments d inner join employees e 
on d.DepartMentId = e.DepartMentId
Group by d.DepartMentId
Having Count(e.EmployeeId)>=1

with DeptStats as
(
	SELECT 
        e.FirstName, 
        d.DepartmentName, 
        e.Salary,
        AVG(e.Salary) OVER(PARTITION BY d.DepartmentID) as AvgSalary,
        COUNT(e.EmployeeID) OVER(PARTITION BY d.DepartmentID) as EmpCount
    FROM Employees e
    JOIN Departments d ON e.DepartmentID = d.DepartmentID 
)

SELECT FirstName, DepartmentName, Salary, AvgSalary
FROM DeptStats
WHERE EmpCount > 1;

-- List all employees and their Manager's name

select 
(e.FirstName + ' ' + e.LastName) as EmployeeName,
(m.FirstName + ' ' + m.LastName) as ManagerName
from employees e left join employees m on e.ManagerId = m.EmployeeId

--3. Find all employees who earn more than the company average.
--Non - Corelated
select * from employees where salary > (
select avg(salary) from employees)

--Find employees who earn more than the average salary of their own department
--Correlated

Select * from employees e1 where e1.Salary >
(
	select avg(salary) from employees  e2
	where e1.DepartMentId = e2.DepartMentId
)

with CTE as (select 
EmployeeId,
FirstName,
Avg(salary) over (partition by departmentId) avgSalary,
salary,
departmentId
from employees)

select EmployeeId,FirstName, Salary,DepartMentId from CTE where salary > avgSalary

---4. Island group
WITH GroupedData AS (
    SELECT 
        EmployeeID,
        AttendanceDate,
        DATEADD(day, -ROW_NUMBER() OVER(PARTITION BY EmployeeID ORDER BY AttendanceDate), AttendanceDate) AS IslandGroup
    FROM Attendance
)
SELECT 
    EmployeeID, IslandGroup,
    MIN(AttendanceDate) AS StartDate, 
    MAX(AttendanceDate) AS EndDate
FROM GroupedData
GROUP BY EmployeeID, IslandGroup;

--5 Delete duplicate

WITH CTE AS (
    SELECT *, 
    ROW_NUMBER() OVER (
        PARTITION BY FirstName, LastName, DepartmentID 
        ORDER BY EmployeeID
    ) AS DuplicateCount
    FROM Employees
)
Select * FROM CTE WHERE DuplicateCount > 0;

--6. Calculating MOM (month over month) growth
SELECT 
    Month,
    TotalRevenue,
    LAG(TotalRevenue) OVER (ORDER BY Month) AS PreviousMonthRevenue,
    (TotalRevenue - LAG(TotalRevenue) OVER (ORDER BY Month)) / 
        LAG(TotalRevenue) OVER (ORDER BY Month) * 100 AS GrowthPercentage
FROM MonthlyRevenue;

--7. : For each employee, list their skills and show how many total skills they have, 
		--but only if they have more than 2 skills.

		With EmployeeSkillStats as
		(
			select
			employeeid,
			skillname,
			count(skillname) over (partition by employeeid) as TotalSkills,
			ProficiencyLevel
			from
			EmployeeSkills
		)
		select employeeid, skillname from EmployeeSkillStats where TotalSkills > 2

--8. Skill Matrix

SELECT EmployeeID, [SQL], [C#], [.NET], [Python]
FROM (
    SELECT EmployeeID, SkillName, ProficiencyLevel
    FROM EmployeeSkills
) AS SourceTable
PIVOT (
    MIN(ProficiencyLevel)
    FOR SkillName IN ([SQL], [C#], [.NET], [Python])
) AS PivotTable;

--9. Calculate Birthdate
select 
Employeeid, FirstName,
((cast(format(GETDATE(),'yyyyMMdd') AS INT))-(cast(format(BirthDate,'yyyyMMdd') AS INT)))/10000 as Age
from employees

--10. Calculating Tenure
SELECT 
    FirstName, 
    HireDate,
    DATEDIFF(year, HireDate, GETDATE()) AS YearsOfTenure,
    -- More precise tenure (months)
    DATEDIFF(month, HireDate, GETDATE()) AS TotalMonths,
	CONCAT(
        DATEDIFF(MONTH, HireDate, GETDATE()) / 12, ' Years, ',
        DATEDIFF(MONTH, HireDate, GETDATE()) % 12, ' Months'
    ) AS TenureFormatted
FROM Employees;

--11. Checking IsNull
-- If Salary is NULL, show 0 instead
SELECT FirstName, ISNULL(Salary, 0) as Salary
FROM Employees;

-- If DepartmentName is NULL, show 'Bench'SELECT 
SELECT 
    FirstName, 
    ISNULL(CAST(ManagerID AS VARCHAR(10)), 'Bench') AS ManagerStatus
FROM Employees;

-- Prevents the query from crashing if TotalBookings is 0
SELECT ISNULL(TotalRevenue / NULLIF(TotalBookings, 0),0) AS RevPerBooking
FROM Sales;

-- Wraps column in a function
SELECT * FROM Sales WHERE MONTH(SaleDate) = 4;

-- Uses a range

SELECT * FROM Sales WHERE SaleDate >= '2026-04-01' AND SaleDate <= '2026-04-30';

-- Check PersonalEmail, then WorkEmail; if both are NULL, show 'No Email'
SELECT FirstName, COALESCE(PersonalEmail, WorkEmail, 'No Email') as ContactInfo
FROM Employees;

SELECT 
    FirstName,
    COALESCE(
        NULLIF(WorkEmail, ''), 
        NULLIF(PersonalEmail, ''), 
        'No Email Provided'
    ) AS PrimaryEmail
FROM Employees;

--12 Retrive common records from two tables
select DepartmentID from Employees
intersect
select DepartmentID from Departments

--13 Find department with highest number of employees
select Top 1 DepartmentID,count(employeeId) as TotalCount from employees group by DepartmentID
order by totalCount desc;

select *,
	departmentId,
	Count(employeeId) over (partition by departmentId) as deptEmployeeCount
from employees;

--14 Find employees who have been in company more than 5 years
select * 
from employees
where datediff(DAY,HireDate,GETDATE())>1825

--15 Second Highest Salary for each department
with CTE AS(
select * ,
Dense_Rank() over (partition by departmentId order by salary desc) as SalaryRank

from Employees)
select * from CTE
where 
SalaryRank = 2

--16 Employee works in multiple departments
SELECT EmployeeID FROM Employees WHERE DepartmentID = 1 -- IT
INTERSECT
SELECT EmployeeID FROM Employees WHERE DepartmentID = 2; -- HR

--17 To find max and min salary in each department
Select Employees.DepartmentId, Departments.DepartmentName, Max(salary), Min(salary) from Employees
Inner Join Departments on Employees.DepartmentID = Departments.DepartmentID
group by Employees.DepartmentID, Departments.DepartmentName;

WITH SalaryStats AS (
    SELECT DepartmentID, MAX(Salary) as MaxSal, MIN(Salary) as MinSal
    FROM Employees
    GROUP BY DepartmentID
)
SELECT d.DepartmentName, s.MaxSal, s.MinSal
FROM Departments d
Left JOIN SalaryStats s ON d.DepartmentID = s.DepartmentID;

--18 Find employees who have been in company for 6 Months
select *
from employees
where datediff(MONTH,HireDate,GETDATE())<6

--19 Top 2 salary for each departments
with CTE AS(
select * ,
Dense_Rank() over (partition by departmentId order by salary desc) as SalaryRank

from Employees)
select * from CTE
where 
SalaryRank < 3

--20 Manager manages atleast one employee
-- We can use order by for aggreate function
--It is optional to write aggregate functions with grouping columns
--You can have columns in your GROUP BY that you choose not to show in your SELECT
select * from Employees M where Exists(
select 1 from Employees E where E.ManagerId = m.ManagerID)

WITH ManagerCounts AS (
    SELECT 
        ManagerID, 
        COUNT(*) OVER(PARTITION BY ManagerID) as SubordinateCount
    FROM Employees
    WHERE ManagerID IS NOT NULL
)
SELECT DISTINCT E.*
FROM Employees E
JOIN ManagerCounts MC ON E.EmployeeID = MC.ManagerID
WHERE MC.SubordinateCount >= 2;

WITH ManagerCounts1 AS (
    SELECT 
        ManagerID, 
        COUNT(*) OVER(PARTITION BY ManagerID) as SubordinateCount
    FROM Employees
    WHERE ManagerID IS NOT NULL
)
sELECT * FROM Employees WHERE ManagerID iN ( SELECT ManagerID FROM ManagerCounts1 WHERE SubordinateCount > 1)

--21. Total revenue for each region in a given quarter
SELECT region, SUM(TotalRevenue) as total_revenue
FROM sales
WHERE DATEPART(quarter, SaleDate) = 2 
  AND DATEPART(year, SaleDate) = 2026
GROUP BY region;

--22. Customers who made a purchase every month for the past year
SELECT customer_id
FROM transactions
-- Filters for records from exactly one year ago today
WHERE transaction_date >= DATEADD(year, -1, GETDATE())
GROUP BY customer_id
-- Counts unique Year-Month combinations to ensure it covers 12 distinct months
HAVING COUNT(DISTINCT FORMAT(transaction_date, 'yyyy-MM')) = 12;

--23. Find employees who have the same manager

SELECT managerid, STRING_AGG(FirstName + ' '+ LastName, ', ') as employees
FROM Employees
GROUP BY managerid
HAVING COUNT(employeeid) > 1;

--24. First and last record for each employee based on 'hire_date'
SELECT employeeid,
       MIN(hiredate) OVER(PARTITION BY employeeid) as first_record,
       MAX(hiredate) OVER(PARTITION BY employeeid) as last_record
FROM employees;

--25. Most recent transaction for each customer
SELECT customer_id, transaction_date, amount
FROM (
    SELECT *, RANK() OVER(PARTITION BY customer_id ORDER BY transaction_date DESC) as rnk
    FROM transactions
) t
WHERE rnk = 1;

--26. Running total of orders for each customer
SELECT customer_id, order_date, 
       SUM(order_amount) OVER(PARTITION BY customer_id ORDER BY order_date) as running_total
FROM orders;

--27. List all employees who are also managers
SELECT DISTINCT (e.FirstName + ' ' + e.LastName) as EmployeeName
FROM Employees e
JOIN employees m ON e.employeeid = m.managerid;

--28. Total number of employees hired per month and year
SELECT YEAR(hiredate) as year, MONTH(hiredate) as month, COUNT(*) as hire_count
FROM employees
GROUP BY YEAR(hiredate), MONTH(hiredate);

--29. Employees who joined in the same month and year
SELECT hiredate, STRING_AGG(e.FirstName + ' ' + e.LastName, ', ') as EmployeeName
FROM employees e
GROUP BY hiredate
HAVING COUNT(*) > 1;

--30. Employees older than the average age of their department
SELECT *
FROM employees e1
WHERE ((cast(format(GETDATE(),'yyyyMMdd') AS INT))-(cast(format(BirthDate,'yyyyMMdd')
AS INT)))/10000 
> (SELECT AVG(((cast(format(GETDATE(),'yyyyMMdd') AS INT))-(cast(format(BirthDate,
'yyyyMMdd') AS INT)))/10000) FROM employees e2 WHERE e1.departmentid = e2.departmentid);

--31. Employee(s) with the longest tenure
SELECT FirstName, hiredate
FROM employees
WHERE hiredate = (SELECT MIN(hiredate) FROM employees);

--32. Average order value by customer for each month
SELECT customer_id, MONTH(order_date) as month, AVG(order_amount)
FROM orders
GROUP BY customer_id, MONTH(order_date);

--33. Find all pairs of products ordered together at least once
SELECT a.product_id, b.product_id
FROM order_items a
JOIN order_items b ON a.order_id = b.order_id AND a.product_id < b.product_id;
