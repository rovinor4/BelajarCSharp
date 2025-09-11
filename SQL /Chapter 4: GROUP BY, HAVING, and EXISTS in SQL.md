# **Chapter 4: GROUP BY, HAVING, and EXISTS in SQL**

## **1. Introduction**

When working with SQL, sometimes you need to **group** data to perform calculations per category.

* `GROUP BY` groups rows that have the same values.
* `HAVING` filters grouped data.
* `EXISTS` checks if a subquery returns any results.

Kita pakai data dari **Bab 3** (`students`, `classes`, `teachers`) dan tambahin tabel **scores** buat latihan.

---

## **2. Preparing Additional Table for Examples**

```sql
CREATE TABLE scores (
    score_id INT PRIMARY KEY,
    student_id INT,
    subject VARCHAR(50),
    score INT
);

INSERT INTO scores (score_id, student_id, subject, score) VALUES
(1, 1, 'Math', 85),
(2, 1, 'Science', 90),
(3, 2, 'Math', 78),
(4, 2, 'Science', 88),
(5, 3, 'Math', 92),
(6, 3, 'Science', 81),
(7, 4, 'Math', 70);
```

---

## **3. GROUP BY**

`GROUP BY` groups data by one or more columns, often used with aggregate functions (`COUNT`, `SUM`, `AVG`, etc.).

Example: Count students per class.

```sql
SELECT classes.class_name, COUNT(students.student_id) AS total_students
FROM classes
LEFT JOIN students ON classes.class_id = students.class_id
GROUP BY classes.class_name;
```

**Result:**

* Each `class_name` appears once with the total number of students.

---

## **4. HAVING**

`HAVING` filters results after grouping (unlike `WHERE` which filters before grouping).

Example: Show classes with more than 1 student.

```sql
SELECT classes.class_name, COUNT(students.student_id) AS total_students
FROM classes
LEFT JOIN students ON classes.class_id = students.class_id
GROUP BY classes.class_name
HAVING COUNT(students.student_id) > 1;
```

**Result:**

* Only classes with more than one student are shown.

---

## **5. EXISTS**

`EXISTS` checks if a subquery returns any result (TRUE/FALSE).

Example: Find students who have a score in "Science".

```sql
SELECT name
FROM students s
WHERE EXISTS (
    SELECT 1
    FROM scores sc
    WHERE sc.student_id = s.student_id
      AND sc.subject = 'Science'
);
```

**Explanation:**

* If the subquery finds at least one matching row, `EXISTS` returns TRUE.

---

## **6. Combining GROUP BY, HAVING, and EXISTS**

Example: Show classes that have students who scored above 85 in Math.

```sql
SELECT classes.class_name, COUNT(students.student_id) AS high_scorers
FROM classes
INNER JOIN students ON classes.class_id = students.class_id
WHERE EXISTS (
    SELECT 1
    FROM scores
    WHERE scores.student_id = students.student_id
      AND scores.subject = 'Math'
      AND scores.score > 85
)
GROUP BY classes.class_name
HAVING COUNT(students.student_id) >= 1;
```

**Result:**

* Only classes where at least one student has scored above 85 in Math are shown.

---

## **7. Best Practices**

1. Use `GROUP BY` for summarizing data.
2. Use `HAVING` to filter grouped results, not raw data.
3. Use `EXISTS` instead of `IN` when you only need to check existence for performance on large datasets.
4. Always pair `GROUP BY` with aggregate functions for meaningful results.
