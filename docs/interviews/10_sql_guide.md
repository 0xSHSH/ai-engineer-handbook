# 🔟 SQL Interview Guide

> Part of the [Interview Handbook](README.md).

## 📑 Contents
- [Joins](#joins)
- [Indexes](#indexes)
- [Normalization](#normalization)
- [Transactions & ACID](#transactions--acid)
- [Locks](#locks)
- [Window Functions](#window-functions)
- [Recursive Queries](#recursive-queries)
- [Query Optimization](#query-optimization)
- [Interview Questions](#interview-questions)

---

## Joins
```sql
-- INNER JOIN: only matching rows in both tables
SELECT o.id, c.name FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;

-- LEFT JOIN: all rows from orders, matched customer data or NULL
SELECT o.id, c.name FROM orders o
LEFT JOIN customers c ON o.customer_id = c.id;

-- SELF JOIN: comparing rows within the same table (e.g., employees and managers)
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```
`FULL OUTER JOIN` returns all rows from both sides (NULL where unmatched) — not supported natively in MySQL, emulate with `LEFT JOIN UNION RIGHT JOIN`.

## Indexes
- A B-tree index turns an O(n) table scan into an O(log n) lookup for equality/range queries on the indexed column(s).
- **Composite indexes**: column order matters — an index on `(a, b)` speeds up queries filtering on `a` alone or `a AND b`, but not on `b` alone (leftmost-prefix rule).
- **Trade-off**: every index speeds up reads but slows down writes (insert/update/delete must maintain the index too) and uses additional storage.
- `EXPLAIN` (or `EXPLAIN ANALYZE`) shows whether a query is actually using an index or falling back to a full table scan.

## Normalization
| Form | Rule | Fixes |
|---|---|---|
| 1NF | Atomic columns, no repeating groups | Multi-valued fields split into rows |
| 2NF | 1NF + no partial dependency on a composite key | Non-key columns must depend on the *whole* key |
| 3NF | 2NF + no transitive dependency | Non-key columns must depend only on the key, not on other non-key columns |

**Denormalization** (deliberately violating normal form) is a valid trade-off for read-heavy systems — trading write complexity/storage for fewer joins and faster reads. Know when to justify it, not just define it.

## Transactions & ACID
- **Atomicity**: all-or-nothing — a transaction either fully commits or fully rolls back.
- **Consistency**: a transaction moves the DB from one valid state to another (constraints/triggers hold before and after).
- **Isolation**: concurrent transactions don't see each other's uncommitted changes (degree controlled by isolation level).
- **Durability**: once committed, changes survive a crash (write-ahead logging).

**Isolation levels** (weakest → strongest): Read Uncommitted → Read Committed → Repeatable Read → Serializable. Higher isolation = fewer anomalies (dirty reads, non-repeatable reads, phantom reads) but more locking/lower concurrency.

## Locks
- **Shared (read) lock**: multiple transactions can hold it simultaneously; blocks writes.
- **Exclusive (write) lock**: only one transaction holds it; blocks both reads and writes depending on isolation level.
- **Deadlock**: two transactions each hold a lock the other needs — DBs detect this and abort one transaction (the "victim"). Mitigate by always acquiring locks in a consistent order across the codebase.
- **Optimistic vs pessimistic locking**: pessimistic locks rows up front (safe, lower concurrency); optimistic checks a version/timestamp column at commit time and retries on conflict (higher concurrency, needs retry logic).

## Window Functions
```sql
-- Running total per customer, ordered by date
SELECT customer_id, order_date, amount,
       SUM(amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS running_total
FROM orders;

-- Rank customers by total spend
SELECT customer_id, total_spend,
       RANK() OVER (ORDER BY total_spend DESC) AS spend_rank
FROM customer_totals;

-- Row number to dedupe, keeping the latest row per group
WITH ranked AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY updated_at DESC) AS rn
  FROM user_events
)
SELECT * FROM ranked WHERE rn = 1;
```
`RANK()` leaves gaps after ties; `DENSE_RANK()` doesn't; `ROW_NUMBER()` never ties.

## Recursive Queries
```sql
-- Employee hierarchy from a manager_id self-reference
WITH RECURSIVE org_chart AS (
  SELECT id, name, manager_id, 1 AS depth
  FROM employees WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id, oc.depth + 1
  FROM employees e
  JOIN org_chart oc ON e.manager_id = oc.id
)
SELECT * FROM org_chart ORDER BY depth;
```

## Query Optimization
- Filter early: `WHERE` before `JOIN`-ing large tables where possible (or trust the optimizer, but verify with `EXPLAIN`).
- Avoid `SELECT *` in production queries — fetch only needed columns to reduce I/O and enable covering indexes.
- `LIMIT` + proper indexing beats fetching everything and filtering in application code.
- Beware of functions on indexed columns in `WHERE` clauses (e.g., `WHERE YEAR(created_at) = 2026`) — this prevents index usage; rewrite as a range (`WHERE created_at >= '2026-01-01' AND created_at < '2027-01-01'`).
- N+1 query problem: fetching a list then querying per-row in a loop — fix with a single JOIN or batched `IN (...)` query.

---

## Interview Questions
1. Write a query to find the second-highest salary per department.
2. Explain the difference between `WHERE` and `HAVING`.
3. What's the difference between a clustered and non-clustered index?
4. When would you denormalize a schema, and what do you give up?
5. Write a query using a window function to compute a 7-day rolling average.
6. How would you find and remove duplicate rows keeping only one copy?

```sql
-- Q1 answer
SELECT department_id, MAX(salary) AS second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees e2 WHERE e2.department_id = employees.department_id)
GROUP BY department_id;
```

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [API Guide](11_api_guide.md).*
