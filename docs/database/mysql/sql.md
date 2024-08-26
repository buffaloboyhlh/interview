# SQL 教程

这里是一个详细的 SQL 教程，包括基本概念、常用操作、复杂查询等内容。希望对你学习和掌握 SQL 有帮助！

---

## **1. SQL 基础概念**

- **SQL (Structured Query Language)**: 用于管理关系型数据库的标准语言。
- **数据库 (Database)**: 存储数据的容器，包含多个表。
- **表 (Table)**: 数据以表格形式存储，由行（记录）和列（字段）组成。
- **列 (Column)**: 表中的字段，定义了数据的类型。
- **行 (Row)**: 表中的一条记录，包含多个字段的数据。

## **2. SQL 常用语句**

### **2.1 创建数据库和表**

- **创建数据库**:
  ```sql
  CREATE DATABASE database_name;
  ```

- **创建表**:
  ```sql
  CREATE TABLE table_name (
      column1 datatype constraints,
      column2 datatype constraints,
      ...
  );
  ```

  **示例**:
  ```sql
  CREATE TABLE employees (
      employee_id INT PRIMARY KEY,
      first_name VARCHAR(50) NOT NULL,
      last_name VARCHAR(50) NOT NULL,
      hire_date DATE
  );
  ```

### **2.2 插入数据**

- **插入单条记录**:
  ```sql
  INSERT INTO table_name (column1, column2, ...)
  VALUES (value1, value2, ...);
  ```

  **示例**:
  ```sql
  INSERT INTO employees (employee_id, first_name, last_name, hire_date)
  VALUES (1, 'John', 'Doe', '2024-08-26');
  ```

- **插入多条记录**:
  ```sql
  INSERT INTO table_name (column1, column2, ...)
  VALUES (value1, value2, ...), (value1, value2, ...), ...;
  ```

  **示例**:
  ```sql
  INSERT INTO employees (employee_id, first_name, last_name, hire_date)
  VALUES (2, 'Jane', 'Smith', '2024-08-27'),
         (3, 'Emily', 'Jones', '2024-08-28');
  ```

### **2.3 查询数据**

- **查询所有记录**:
  ```sql
  SELECT * FROM table_name;
  ```

- **查询特定列**:
  ```sql
  SELECT column1, column2 FROM table_name;
  ```

- **查询带条件**:
  ```sql
  SELECT column1, column2
  FROM table_name
  WHERE condition;
  ```

  **示例**:
  ```sql
  SELECT first_name, last_name
  FROM employees
  WHERE hire_date > '2024-01-01';
  ```

- **排序查询结果**:
  ```sql
  SELECT column1, column2
  FROM table_name
  ORDER BY column1 [ASC|DESC];
  ```

  **示例**:
  ```sql
  SELECT first_name, hire_date
  FROM employees
  ORDER BY hire_date DESC;
  ```

- **限制查询结果数量**:
  ```sql
  SELECT column1, column2
  FROM table_name
  LIMIT number;
  ```

  **示例**:
  ```sql
  SELECT * FROM employees
  LIMIT 5;
  ```

### **2.4 更新数据**

- **更新记录**:
  ```sql
  UPDATE table_name
  SET column1 = value1, column2 = value2, ...
  WHERE condition;
  ```

  **示例**:
  ```sql
  UPDATE employees
  SET hire_date = '2024-09-01'
  WHERE employee_id = 1;
  ```

### **2.5 删除数据**

- **删除记录**:
  ```sql
  DELETE FROM table_name
  WHERE condition;
  ```

  **示例**:
  ```sql
  DELETE FROM employees
  WHERE employee_id = 3;
  ```

- **删除表**:
  ```sql
  DROP TABLE table_name;
  ```

- **删除数据库**:
  ```sql
  DROP DATABASE database_name;
  ```

### **2.6 创建索引**

- **创建索引**:
  ```sql
  CREATE INDEX index_name
  ON table_name (column1, column2, ...);
  ```

  **示例**:
  ```sql
  CREATE INDEX idx_last_name
  ON employees (last_name);
  ```

- **删除索引**:
  ```sql
  DROP INDEX index_name
  ON table_name;
  ```

  **示例**:
  ```sql
  DROP INDEX idx_last_name
  ON employees;
  ```

## **3. 复杂查询**

### **3.1 联接（JOIN）**

- **内联接 (INNER JOIN)**:
  ```sql
  SELECT columns
  FROM table1
  INNER JOIN table2
  ON table1.column = table2.column;
  ```

  **示例**:
  ```sql
  SELECT employees.first_name, departments.department_name
  FROM employees
  INNER JOIN departments
  ON employees.department_id = departments.department_id;
  ```

- **左联接 (LEFT JOIN)**:
  ```sql
  SELECT columns
  FROM table1
  LEFT JOIN table2
  ON table1.column = table2.column;
  ```

  **示例**:
  ```sql
  SELECT employees.first_name, departments.department_name
  FROM employees
  LEFT JOIN departments
  ON employees.department_id = departments.department_id;
  ```

### **3.2 子查询**

- **在 SELECT 语句中使用子查询**:
  ```sql
  SELECT column1
  FROM table_name
  WHERE column2 = (SELECT column2 FROM table_name WHERE condition);
  ```

  **示例**:
  ```sql
  SELECT first_name
  FROM employees
  WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'Sales');
  ```

- **在 FROM 子句中使用子查询**:
  ```sql
  SELECT *
  FROM (SELECT column1, column2 FROM table_name WHERE condition) AS alias_name;
  ```

  **示例**:
  ```sql
  SELECT *
  FROM (SELECT first_name, hire_date FROM employees WHERE hire_date > '2024-01-01') AS recent_employees;
  ```

### **3.3 聚合函数**

- **计算总和**:
  ```sql
  SELECT SUM(column_name) FROM table_name;
  ```

- **计算平均值**:
  ```sql
  SELECT AVG(column_name) FROM table_name;
  ```

- **计算最大值**:
  ```sql
  SELECT MAX(column_name) FROM table_name;
  ```

- **计算最小值**:
  ```sql
  SELECT MIN(column_name) FROM table_name;
  ```

- **计算记录数量**:
  ```sql
  SELECT COUNT(column_name) FROM table_name;
  ```

  **示例**:
  ```sql
  SELECT COUNT(*) FROM employees;
  ```

### **3.4 分组与排序**

- **分组数据**:
  ```sql
  SELECT column_name, COUNT(*)
  FROM table_name
  GROUP BY column_name;
  ```

  **示例**:
  ```sql
  SELECT department_id, COUNT(*)
  FROM employees
  GROUP BY department_id;
  ```

- **对分组数据排序**:
  ```sql
  SELECT column_name, COUNT(*)
  FROM table_name
  GROUP BY column_name
  ORDER BY COUNT(*) DESC;
  ```

  **示例**:
  ```sql
  SELECT department_id, COUNT(*)
  FROM employees
  GROUP BY department_id
  ORDER BY COUNT(*) DESC;
  ```

### **3.5 使用条件表达式**

- **CASE 语句**:
  ```sql
  SELECT column_name,
         CASE
             WHEN condition1 THEN result1
             WHEN condition2 THEN result2
             ELSE result3
         END AS alias_name
  FROM table_name;
  ```

  **示例**:
  ```sql
  SELECT first_name,
         CASE
             WHEN hire_date < '2024-01-01' THEN 'Experienced'
             ELSE 'New'
         END AS experience_level
  FROM employees;
  ```

---

这份 SQL 教程涵盖了 SQL 的基本用法、复杂查询和实际应用。通过掌握这些内容，你可以高效地管理和操作关系型数据库，进行各种数据分析和处理。