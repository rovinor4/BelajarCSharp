# **Chapter 3: SQL JOIN Operations**

## **1. Introduction**

`JOIN` in SQL is used to combine rows from two or more tables based on a related column. The most common types are:

* **INNER JOIN** – returns only matching rows.
* **LEFT JOIN** – returns all rows from the left table, plus matched rows from the right table.
* **RIGHT JOIN** – returns all rows from the right table, plus matched rows from the left table.
* **JOIN with 3 tables** – combines data from three related tables.

---

## **2. Preparing Sample Tables**

```sql
-- Students Table
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    name VARCHAR(50),
    class_id INT
);

INSERT INTO students (student_id, name, class_id) VALUES
(1, 'John Doe', 1),
(2, 'Jane Smith', 1),
(3, 'Michael Brown', 2),
(4, 'Emily Davis', NULL);

-- Classes Table
CREATE TABLE classes (
    class_id INT PRIMARY KEY,
    class_name VARCHAR(50),
    teacher_id INT
);

INSERT INTO classes (class_id, class_name, teacher_id) VALUES
(1, 'Mathematics', 101),
(2, 'Science', 102),
(3, 'History', 103);

-- Teachers Table
CREATE TABLE teachers (
    teacher_id INT PRIMARY KEY,
    teacher_name VARCHAR(50)
);

INSERT INTO teachers (teacher_id, teacher_name) VALUES
(101, 'Mr. Johnson'),
(102, 'Ms. Lee'),
(103, 'Mr. White');
```

---

## **3. INNER JOIN**

Returns only rows where there is a match in both tables.

```sql
SELECT students.name, classes.class_name
FROM students
INNER JOIN classes ON students.class_id = classes.class_id;
```

**Result:**

* Only students assigned to a class are shown.

---

## **4. LEFT JOIN**

Returns all rows from the left table, even if there is no match in the right table.

```sql
SELECT students.name, classes.class_name
FROM students
LEFT JOIN classes ON students.class_id = classes.class_id;
```

**Result:**

* All students are shown. If no class is assigned, `NULL` appears for `class_name`.

---

## **5. RIGHT JOIN**

Returns all rows from the right table, even if there is no match in the left table.

```sql
SELECT students.name, classes.class_name
FROM students
RIGHT JOIN classes ON students.class_id = classes.class_id;
```

**Result:**

* All classes are shown, even if there is no student assigned to them.

---

## **6. JOIN with 3 Tables**

Combines data from `students`, `classes`, and `teachers`.

```sql
SELECT students.name AS student_name, 
       classes.class_name, 
       teachers.teacher_name
FROM students
INNER JOIN classes ON students.class_id = classes.class_id
INNER JOIN teachers ON classes.teacher_id = teachers.teacher_id;
```

**Result:**

* Shows each student with their class and teacher’s name.

---

## **7. Best Practices**

1. Always use table aliases for readability when working with multiple tables.
2. Use `INNER JOIN` when you only want matched data.
3. Use `LEFT` or `RIGHT JOIN` when unmatched rows are also important.
4. Avoid unnecessary joins for better performance.

