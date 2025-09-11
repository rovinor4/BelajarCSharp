# **Chapter 2: SQL Filtering, Aliases, and Aggregate Functions**

## **1. Introduction**

In SQL, filtering and aggregation are essential for retrieving meaningful data from a database. This chapter covers the `LIKE`, `WHERE IN`, `BETWEEN`, `ALIAS`, and key aggregate functions: `AVG`, `SUM`, `COUNT`, `MIN`, and `MAX`.

---

## **2. Filtering with LIKE**

`LIKE` is used for pattern matching in text data.

```sql
SELECT name
FROM students
WHERE name LIKE 'J%';
```

**Explanation:**

* `J%` matches any name starting with **J**.
* `%` means zero or more characters.
* `_` (underscore) means exactly one character.

Example:

```sql
-- Matches any name that has 'an' anywhere
WHERE name LIKE '%an%';
```

---

## **3. Filtering with WHERE IN**

`WHERE IN` checks if a value matches any value in a list.

```sql
SELECT name, major
FROM students
WHERE major IN ('Computer Science', 'Data Science');
```

**Explanation:**

* Easier than using multiple `OR` conditions.

---

## **4. Filtering with BETWEEN**

`BETWEEN` selects values within a given range (inclusive).

```sql
SELECT name, age
FROM students
WHERE age BETWEEN 18 AND 25;
```

**Explanation:**

* Includes both boundary values (18 and 25).

---

## **5. Using ALIAS**

`ALIAS` gives a temporary name to a column or table for readability.

```sql
SELECT name AS student_name, major AS field
FROM students;
```

**Explanation:**

* `AS` creates a more descriptive column name in the result.

---

## **6. Aggregate Functions**

### **6.1 AVG**

Calculates the average value.

```sql
SELECT AVG(age) AS average_age
FROM students;
```

### **6.2 SUM**

Adds up all values in a column.

```sql
SELECT SUM(salary) AS total_salary
FROM teachers;
```

### **6.3 COUNT**

Counts the number of rows (or non-null values).

```sql
SELECT COUNT(*) AS total_students
FROM students;
```

### **6.4 MIN**

Finds the smallest value.

```sql
SELECT MIN(age) AS youngest_student
FROM students;
```

### **6.5 MAX**

Finds the largest value.

```sql
SELECT MAX(age) AS oldest_student
FROM students;
```

---

## **7. Best Practices**

1. Use `LIKE` with wildcards carefully, as it can slow down queries on large datasets.
2. Use `WHERE IN` instead of multiple `OR` for better readability.
3. Remember that `BETWEEN` is inclusive.
4. Always use descriptive aliases for clarity.
5. Combine aggregate functions with `GROUP BY` to summarize data per category.
