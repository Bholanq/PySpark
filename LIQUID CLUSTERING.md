We can enable Liquid Clustering when creating a table, or by altering a pre-existing table using the `CLUSTER BY` command.
# How to enable it
When creating a table:
```
CREATE TABLE orders (
order_id BIGINT,   
customer_id BIGINT,   
country STRING,   
amount DOUBLE)
USING DELTA
CLUSTER BY (customer_id);
```
Or:
```
ALTER TABLE orders
CLUSTER BY (customer_id);
```

Databricks **does not immediately reorganize all the existing data**.

What this statement does is:

> **It updates the table metadata to tell Databricks which columns should be used for clustering going forward.**

# ZORDER's approach

ZORDER improves data skipping by rewriting the table so that related values are physically colocated.

Example:

```
OPTIMIZE ordersZORDER BY (customer_id);
```

Databricks rewrites the files.

Before:

```
File 1 → customer_ids 1–10000File 2 → customer_ids 1–10000File 3 → customer_ids 1–10000
```

After:

```
File 1 → customer_ids 1–1000File 2 → customer_ids 1001–2000File 3 → customer_ids 2001–3000
```

The problem is that **every time new data arrives**, this organization gradually deteriorates.

---

# The maintenance problem

Suppose you ingest 100 GB every day.

After a month:

```
Existing data: 3 TBNew data:      3 TB
```

To maintain ZORDER performance, you repeatedly run:

```
OPTIMIZE ordersZORDER BY (customer_id);
```

This can rewrite terabytes of data.

That means:

- More compute,
- Longer maintenance windows,
- Higher costs.

---

# Liquid Clustering's idea

Instead of saying:

> "Reorganize the entire table every time,"

Liquid Clustering says:

> "Maintain clustering continuously and incrementally."