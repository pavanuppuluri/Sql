# Sql

**Data Definition Language (DDL)**

Here’s the content formatted into a two-column markup table:

| Command   | Description                                                               |
|-----------|---------------------------------------------------------------------------|
| CREATE    | Creates a new table, a view of a table, or other object in the database. |
| ALTER     | Modifies an existing database object, such as a table.                   |
| DROP      | Deletes an entire table, a view of a table, or other objects in the database. |
| TRUNCATE  | Truncates the entire table in one go.                                    |

**Data Manipulation Language (DML)**
Here’s the additional content formatted into a two-column markup table:

| Command  | Description                                             |
|----------|---------------------------------------------------------|
| SELECT   | Retrieves certain records from one or more tables.     |
| INSERT   | Creates a new record.                                   |
| UPDATE   | Modifies existing records.                              |
| DELETE   | Deletes records.                                       |

**Data Control Language (DCL)**
Data Control Language (DCL) is a computer programming language which is used to control access 
to data stored in a database.

| Command  | Description                                             |
|----------|---------------------------------------------------------|
| GRANT   | Gives a privilege to user     |
| REVOKE   | Takes back privileges granted from user.                |


# SQL Queries with Examples

Below are various SQL queries with examples, including 
- joins,
- `UNION`,
- `GROUP BY`,
- `HAVING`,
- `LIKE`,
- `IN`, etc.

The following sample tables are assumed:

## Tables:

- **employees**:
  - `employee_id` (Primary Key)
  - `first_name`
  - `last_name`
  - `department_id`
  
- **departments**:
  - `department_id` (Primary Key)
  - `department_name`

---

## 1. **INNER JOIN** (Returns records that have matching values in both tables)

#### Query: Get the names of employees and their respective department names.
```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id;
```

### 2. LEFT JOIN (Returns all records from the left table and matched records from the right table)

#### Query: Get all employees and their department names (including employees who may not belong to a department).

```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id;
```

### 3. RIGHT JOIN (Returns all records from the right table and matched records from the left table)

#### Query: Get all departments and their respective employees (including departments with no employees).

```sql
SELECT d.department_name, e.first_name, e.last_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id;
```

### 4. FULL OUTER JOIN (Returns all records when there is a match in either the left or right table)

#### Query: Get all employees and departments (even if there is no match).

```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.department_id;
```

### 5. UNION (Combines the result sets of two or more SELECT statements)

#### Query: Get a list of all employee names and department names from different sources.

```sql
SELECT first_name, last_name
FROM employees
UNION
SELECT department_name, NULL AS last_name
FROM departments;
```

### 6. GROUP BY (Groups rows that have the same values into summary rows)

#### Query: Get the count of employees in each department.

```sql
SELECT d.department_name, COUNT(e.employee_id) AS num_employees
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name;
```

### 7. HAVING (Used with GROUP BY to filter groups)

#### Query: Get departments that have more than 5 employees.

```sql
SELECT d.department_name, COUNT(e.employee_id) AS num_employees
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
HAVING COUNT(e.employee_id) > 5;
```

### 8. LIKE (Search for a specified pattern)

#### Query: Get all employees whose last name starts with "S".

```sql
SELECT first_name, last_name
FROM employees
WHERE last_name LIKE 'S%';
```

#### Query: Get all employees whose first name contains "John".

```sql
SELECT first_name, last_name
FROM employees
WHERE first_name LIKE '%John%';
```

### 9. IN (Checks if a value is within a list of values)

#### Query: Get all employees who belong to either the "HR" or "Sales" department.

```sql
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
WHERE d.department_name IN ('HR', 'Sales');
```

### 10. BETWEEN (Filters the result based on a range)

#### Query: Get employees who joined between January 1, 2020, and December 31, 2021.

```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE hire_date BETWEEN '2020-01-01' AND '2021-12-31';
```

### 11. ORDER BY (Sorts the result set)

#### Query: Get all employees sorted by their last name in ascending order.

```sql
SELECT first_name, last_name
FROM employees
ORDER BY last_name ASC;
```

#### Query: Get all employees sorted by hire date in descending order.

```sql
SELECT first_name, last_name, hire_date
FROM employees
ORDER BY hire_date DESC;
```

### 12. DISTINCT (Removes duplicate records)

#### Query: Get a list of distinct department names.

```sql
SELECT DISTINCT department_name
FROM departments;
```

### 13. Subquery (Query inside another query)

#### Query: Get employees who work in the same department as "John Doe".

```sql
SELECT first_name, last_name
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM employees
    WHERE first_name = 'John' AND last_name = 'Doe'
);
```

### 14. CASE (Used for conditional logic in SELECT statements)

#### Query: Get a list of employees and a "status" indicating whether their hire date is more than 5 years ago.

```sql
SELECT first_name, last_name,
       CASE 
           WHEN hire_date <= DATE_SUB(CURDATE(), INTERVAL 5 YEAR) THEN 'Veteran'
           ELSE 'New Hire'
       END AS status
FROM employees;
```

### 15. Self Join (Joining a table with itself)

#### Query: Get employees and their managers (assuming each employee has a manager, represented by manager_id in the employees table).

```sql
SELECT e1.first_name AS employee_name, e2.first_name AS manager_name
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.employee_id;
```

### 16. INNER JOIN with Multiple Tables

#### Query: Get a list of employees, their department names, and the location of the department.

```sql
SELECT e.first_name, e.last_name, d.department_name, l.location
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
INNER JOIN locations l ON d.location_id = l.location_id;
```

### 17. COUNT, SUM, AVG, MIN, MAX (Aggregate functions)

#### Query: Get the total salary, average salary, highest, and lowest salary in the company.

```sql
SELECT 
    COUNT(employee_id) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary
FROM employees;
```

### 18. DELETE with WHERE (Deleting specific rows)

#### Query: Delete an employee by their employee ID.

```sql
DELETE FROM employees
WHERE employee_id = 123;
```

### 19. UPDATE with WHERE (Updating specific rows)

#### Query: Update the department of an employee with employee_id 123.

```sql
UPDATE employees
SET department_id = 2
WHERE employee_id = 123;
```

**ALTER**

The ALTER TABLE statement is used to alter the structure of a table. 
For instance, you can add, drop, and modify the data of a column using this statement. 
Following is the syntax −

![image](https://github.com/user-attachments/assets/04224e88-c595-425a-be8b-bacc46491212)

![image](https://github.com/user-attachments/assets/64f030f4-a8dc-4ab0-bf06-418bcb565cd8)

![image](https://github.com/user-attachments/assets/3e7cb3b7-e265-41b1-927e-15cd8d887857)

**DISTINCT**
The DISTINCT clause in a database is used to identify the non-duplicate data from a column. 

![image](https://github.com/user-attachments/assets/ce0cd581-f3ef-45a3-980c-a982ea51ab04)

**IN clause**

![image](https://github.com/user-attachments/assets/35ab5f7d-e8fb-4249-811f-35d41663caa7)

**BETWEEN clause**

![image](https://github.com/user-attachments/assets/4e70a904-412c-4a0f-a2bb-78de48c5183d)

**ORDER BY clause**

![image](https://github.com/user-attachments/assets/23b20063-7cfa-4783-bd53-3bd68c738ce0)

**GROUP BY clause**

The GROUP BY Clause is used to group the values of a column together.

![image](https://github.com/user-attachments/assets/abf38f75-5ace-44dc-a5a4-4e6ffec40960)

![image](https://github.com/user-attachments/assets/a2deb685-7044-4e3d-a704-85e3c0b5980a)



































