This statement is **correct**:

> "VACUUM deletes files that have not been part of the Delta table's active state for at least the retention period."


![[Pasted image 20260615181134.png|423]]
Suppose you have a Delta table:
```
sales/├── _delta_log/├── part-001.parquet├── part-002.parquet└── part-003.parquet
```
Now you run:
```
DELETE FROM salesWHERE customer_id = 100;
```
Does Delta modify `part-001.parquet` directly?
**No.**
Delta Lake is **immutable**.
Instead, it:
1. Creates new Parquet files with the updated data.
2. Updates `_delta_log` to point to the new files.
3. Marks the old files as **removed**.

## Example
Before DELETE:
```
sales/├── part-A.parquet├── part-B.parquet└── _delta_log/
```
After DELETE:
```
sales/├── part-A.parquet      ← old├── part-B.parquet      ← old├── part-C.parquet      ← new├── part-D.parquet      ← new└── _delta_log/
```
The Delta log says:
```
Current version:    Use C and DIgnore A and B
```
However:

> **A and B still physically exist in storage.**

# Why keep old files?

Because Delta Lake supports:

- **Time Travel**
- Rollback
- Concurrent reads and writes

For example:

```
SELECT *FROM sales VERSION AS OF 5;
```

might need those old files.

# What does VACUUM do?

```
VACUUM sales;
```

Delta:

```
Finds files no longer referenced by the Delta log
↓
Checks whether the retention period has expired
↓
Permanently deletes them from storage
```
# Default retention period

By default:

```
7 days (168 hours)
```
So:
```
VACUUM sales;
```
is equivalent to:
```
VACUUM sales RETAIN 168 HOURS;
```

# Example timeline
Day 1:
```
Version 1
Uses:AB
```
Day 2:
```
DELETE 
...Version 2
Uses:CD
A and B become obsolete.
```
Day 5:
```
VACUUM sales;
```
Result:
```
A and B are only 3 days old.They are retained.
```
Day 10:
```
VACUUM sales;
```
Result:
```
A and B are now older than 7 days.They are physically deleted.
```



Without VACUUM:
```
Storage keeps growing forever.
```
Because every:
```
DELETEUPDATEMERGEOPTIMIZE
```
creates new files.