![[Pasted image 20260618124437.png|422]]

- TEMP VIEW
- GLOBAL TEMP VIEW
- PERMANENT VIEW

# 1. Temporary View
A temporary view exists only in the current notebook/session.

## Create

```
CREATE OR REPLACE TEMP VIEW emp_view ASSELECT *FROM employees;
```

or

```
df.createOrReplaceTempView("emp_view")
```

---

## Query

```
SELECT * FROM emp_view;
```

Works only within the current Spark session.

---

## What happens when session ends?

```
Notebook closes      ↓Spark session ends      ↓View disappears
```

---

### Example

Notebook A:

```
CREATE TEMP VIEW sales_view ASSELECT * FROM sales;
```

Works:

```
SELECT * FROM sales_view;
```

After cluster restart:

```
SELECT * FROM sales_view;
```

Error:

```
TABLE_OR_VIEW_NOT_FOUND
```

---

# 2. Global Temporary View

Available across multiple notebooks/sessions connected to the same Spark application.

## Create

```
CREATE GLOBAL TEMP VIEW sales_view ASSELECT * FROM sales;
```

or

```
df.createOrReplaceGlobalTempView("sales_view")
```

---

## Query

Must use the special database:

```
SELECT *FROM global_temp.sales_view;
```

Notice:

```
global_temp
```

is mandatory.

---

### Example

Notebook A:

```
CREATE GLOBAL TEMP VIEW sales_view ASSELECT * FROM sales;
```

Notebook B:

```
SELECT * FROM global_temp.sales_view;
```

Works.

---

### What happens after cluster restart?

```
Cluster stops     ↓Spark application ends     ↓View disappears
```

---

# 3. Permanent View

Stored in Unity Catalog or Hive Metastore.

## Create

```
CREATE VIEW sales_view ASSELECT *FROM sales;
```

---

## Query

```
SELECT * FROM sales_view;
```

---

## What happens after cluster restart?

Still exists.

```
Cluster restart      ↓View remains
```

because metadata is stored in the catalog.

---

## Example

```
CREATE VIEW active_customers ASSELECT *FROM customersWHERE status='ACTIVE';
```

Users can query:

```
SELECT *FROM active_customers;
```

months later.

---

# Visual Comparison

### Temporary View

```
Notebook Session      │      └── temp_view
```

Session ends:

```
temp_view gone
```

---

### Global Temporary View

```
Cluster   │   ├── Notebook A   ├── Notebook B   └── global_temp.sales_view
```

Cluster ends:

```
sales_view gone
```

---

### Permanent View

```
Unity Catalog     │     └── sales_view
```

Cluster ends:

```
sales_view still exists
```

---

# Logical View vs Materialized View

Databricks also supports materialized views in Unity Catalog (depending on your workspace/features).

---

## Standard View (Logical View)

```
CREATE VIEW sales_summary ASSELECT region,       SUM(amount)FROM salesGROUP BY region;
```

No data stored.

When queried:

```
SELECT * FROM sales_summary;
```

Spark reruns the underlying query.

---

## Materialized View

```
CREATE MATERIALIZED VIEW sales_summary_mv ASSELECT region,       SUM(amount)FROM salesGROUP BY region;
```

Result is physically stored.

```
Query once      ↓Store result      ↓Future reads are faster
```

Useful for expensive aggregations.