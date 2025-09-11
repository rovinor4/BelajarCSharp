# **Chapter 5: Subqueries, UNION, and CASE in SQL**

## **1. Introduction**

This chapter covers:

* **Subqueries** → Query inside another query.
* **UNION** → Combine results from multiple queries.
* **CASE** → Conditional logic in SQL.

Kita akan buat tabel tambahan bernama **orders** dan **payments** untuk latihan.

---

## **2. Preparing Sample Tables**

```sql
-- Orders Table
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    student_id INT,
    product VARCHAR(50),
    amount INT
);

INSERT INTO orders (order_id, student_id, product, amount) VALUES
(1, 1, 'Book', 50),
(2, 1, 'Pen', 5),
(3, 2, 'Bag', 40),
(4, 3, 'Laptop', 700),
(5, 4, 'Notebook', 15);

-- Payments Table
CREATE TABLE payments (
    payment_id INT PRIMARY KEY,
    order_id INT,
    payment_status VARCHAR(20)
);

INSERT INTO payments (payment_id, order_id, payment_status) VALUES
(1, 1, 'Paid'),
(2, 2, 'Paid'),
(3, 3, 'Unpaid'),
(4, 4, 'Paid'),
(5, 5, 'Unpaid');
```

---

## **3. Subqueries**

A **subquery** is a query inside another query.
Example: Find students who have placed an order above \$100.

```sql
SELECT name
FROM students
WHERE student_id IN (
    SELECT student_id
    FROM orders
    WHERE amount > 100
);
```

**Explanation:**

* Inner query selects `student_id` from orders above \$100.
* Outer query retrieves names from `students`.

---

## **4. UNION**

`UNION` combines results from two queries (removes duplicates by default).
Example: List all product names and teacher names together.

```sql
SELECT product AS name
FROM orders
UNION
SELECT teacher_name AS name
FROM teachers;
```

**Explanation:**

* Both queries must return the same number of columns and compatible data types.
* Use `UNION ALL` to keep duplicates.

---

## **5. CASE**

`CASE` allows conditional logic inside SQL queries.
Example: Categorize order amounts.

```sql
SELECT order_id, product, amount,
    CASE
        WHEN amount >= 500 THEN 'High Value'
        WHEN amount >= 100 THEN 'Medium Value'
        ELSE 'Low Value'
    END AS category
FROM orders;
```

**Explanation:**

* Works like an IF-ELSE statement in SQL.
* Returns a label based on `amount` value.

---

## **6. Combining Subquery + CASE**

Example: Show each student’s total spending and mark if they have unpaid orders.

```sql
SELECT s.name,
       (SELECT SUM(amount)
        FROM orders o
        WHERE o.student_id = s.student_id) AS total_spent,
       CASE
           WHEN EXISTS (
               SELECT 1
               FROM orders o
               JOIN payments p ON o.order_id = p.order_id
               WHERE o.student_id = s.student_id
                 AND p.payment_status = 'Unpaid'
           ) THEN 'Has Unpaid Orders'
           ELSE 'All Paid'
       END AS payment_status
FROM students s;
```

---

## **7. Best Practices**

1. Keep subqueries simple to avoid performance issues.
2. Use `UNION` only when combining datasets with the same structure.
3. Use `CASE` for readable conditional logic in SELECT statements.
4. Prefer `EXISTS` for checking data presence in subqueries.
