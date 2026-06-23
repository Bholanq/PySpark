Because Parquet files are immutable.

| Category        | Statements                                        |
| --------------- | ------------------------------------------------- |
| Create          | `CREATE TABLE`, `CTAS`, `CREATE OR REPLACE TABLE` |
| Insert          | `INSERT INTO`, `INSERT OVERWRITE`                 |
| Modify          | `UPDATE`, `DELETE`, `MERGE`                       |
| Ingestion       | `COPY INTO`                                       |
| Export          | `UNLOAD`                                          |
| History         | `DESCRIBE HISTORY`                                |
| Time Travel     | `VERSION AS OF`, `TIMESTAMP AS OF`                |
| Recovery        | `RESTORE TABLE`                                   |
| Performance     | `OPTIMIZE`, `ZORDER`, `CLUSTER BY`                |
| Storage Cleanup | `VACUUM`                                          |
| Metadata        | `DESCRIBE DETAIL`, `SHOW CREATE TABLE`            |
| Replication     | `SHALLOW CLONE`, `DEEP CLONE`                     |
| Governance      | Constraints, Generated Columns                    |
| Administration  | `ALTER TABLE`, `DROP TABLE`                       |


| Statement           | Purpose                                                                  |
| ------------------- | ------------------------------------------------------------------------ |
| `CACHE SELECT`      | Cache query results in memory                                            |
| `CONVERT TO DELTA`  | Convert Parquet tables/files to Delta                                    |
| `DESCRIBE HISTORY`  | View transaction history                                                 |
| `FSCK REPAIR TABLE` | Repair metadata inconsistencies                                          |
| `GENERATE`          | Create manifest files for external engines. eg. Other than apache spark. |
| `OPTIMIZE`          | Compact files and improve performance                                    |
| `REORG TABLE`       | Rewrite active files physically + DELETION VECTORS                       |
| `RESTORE`           | Roll back a table to an earlier version                                  |
| `VACUUM`            | Delete obsolete files                                                    |

### FSCK REPAIR TABLE
- Only used when data is manually deleted in Cloud Obj Storage
### Before FSCK
Storage:
```
FileA.parquet
FileC.parquet
```
Delta Log:
```
FileA.parquet
FileB.parquet
FileC.parquet
```

Delta Query:

```
SELECT *FROM sales;
```
Fails.
`FilenotFoundException`

```
### Run

```
FSCK REPAIR TABLE sales;
```

FSCK discovers:
```
FileB.parquet is missing.
```

It removes that reference.

Delta Log becomes effectively:
```
FileA.parquetFileC.parquet
```
Queries work again.
```

# MSCK REAPIR TABLE - Adds missing metadata
# REORG
# 1. Delta files are immutable

Suppose you have this Delta table:

|id|name|
|---|---|
|1|Alice|
|2|Bob|
|3|Charlie|

Physically, it may look like:

```
File A.parquet
```

containing:

```
1 Alice2 Bob3 Charlie
```

---

# 2. What happens during DELETE?

Suppose you run:

```
DELETE FROM customersWHERE id = 2;
```

You might imagine Delta going into File A and removing Bob.

It **doesn't**.

Because Parquet files are immutable.

Instead, Delta does something conceptually like this:

```
Old File A:1 Alice2 Bob3 Charlie↓New File B:1 Alice3 Charlie
```

Then the Delta log changes from:

```
Version 1:Use File A
```

to:

```
Version 2:Use File BIgnore File A
```

---

# 3. What if Deletion Vectors are enabled?

Modern Delta Lake supports **Deletion Vectors (DVs)**.

Instead of rewriting File A immediately, Delta can do this:

```
File A:1 Alice2 Bob3 CharlieDeletion Vector:Mark row #2 as deleted
```

The Delta log says:

```
Version 2:Use File ABut skip Bob using the DV
```

---

## Why is this useful?

Because rewriting large files is expensive.

With DVs:

```
DELETEUPDATEMERGE
```

become much faster.

---

# 4. The problem with DVs

Eventually, your table might look like this:

```
File A100 million rowsDeletion Vector:5 million deleted rows
```

Queries now have to do:

```
Read File A↓Read DV metadata↓Filter deleted rows
```

The data is **logically deleted**, but not **physically removed**.

---

# 5. What REORG TABLE does

`REORG TABLE` says:

> "Rewrite the active data files so that these logical deletions become physically applied."

---

## Syntax

```
REORG TABLE customers APPLY (PURGE);
```

---

# Example

Before REORG:

```
File A----------------1 Alice2 Bob3 CharlieDeletion Vector:Delete row 2
```

Current view of the table:

|id|name|
|---|---|
|1|Alice|
|3|Charlie|

---

After:

```
REORG TABLE customers APPLY (PURGE);
```

Delta rewrites the file:

```
File B---------------1 Alice3 Charlie
```

and updates the Delta log:

```
Version 3:Use File B
```

File A becomes obsolete.

---

# 6. Does REORG delete File A?

**No.**

This is a crucial point.

After REORG:

```
File A  ← obsoleteFile B  ← active
```

File A still physically exists.

Why?

Because:

```
Time TravelConcurrent transactions
```

might still need it.

---

# 7. What removes File A?

```
VACUUM customers;
```

after the retention period.