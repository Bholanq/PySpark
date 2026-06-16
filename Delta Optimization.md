
## 1. Optimize command 

- In **PySpark / Spark SQL**, the `OPTIMIZE` command is used mainly with **Delta tables** (Databricks / Delta Lake). It helps **compact small files into larger ones**, improving query performance.
- ![[Pasted image 20260406081151.png|510]]

## 2. ZORDER(ZORDERING, DATA SKIPPING, PRUNING)

![[Pasted image 20260406081653.png|434]]

**Z-Ordering** (or **ZORDER**) in Spark/Delta Lake is a technique used during `OPTIMIZE` to **physically reorganize data in files based on one or more columns**, so queries that filter on those columns run much faster.

Example:

Query:

Suppose the data is initially randomly distributed. Zordering will sort them first based on the specified column.

`WHERE customer_id = 123

Spark checks metadata:
- file1 → range (1–100) ❌ skip
- file2 → range (101–200) ✅ read
- file3 → range (201–300) ❌ skip
So instead of scanning **all files**, it scans only **relevant ones**.
This is called **Data Skipping**.


## Liquid Clustering 


- Dynamically clusters data based on chosen columns
- Automatically maintains clustering over time
- Works incrementally (no full rewrite like OPTIMIZE)
Liquid Clustering is **incremental** meaning the data has to clustered every time there is a change in the data or the new data is added to the original source. 


```
CREATE TABLE orders (
    order_id INT,
    customer_id INT,
    order_date DATE,
    amount DOUBLE
)
USING DELTA
CLUSTER BY (customer_id, order_date);
```

```
ALTER TABLE orders
CLUSTER BY (customer_id, order_date);
```



- **Partitioning** → divide data into folders
- **Z-Ordering** → rearrange data manually
- **Liquid Clustering** → self-organizing system