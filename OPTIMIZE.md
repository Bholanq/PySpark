
`OPTIMIZE` is used to **compact many small files into fewer larger files**.
## Why is it needed?
When you continuously write data into a Delta table (streaming jobs, incremental loads, etc.), you often end up with thousands of small files.
For example:
```
sales/
├── file1.parquet (10 MB)
├── file2.parquet (5 MB)
├── file3.parquet (8 MB)
...
├── file5000.parquet (2 MB)
```

When Spark reads this table:
- It must open thousands of files.
- Metadata operations increase.
- Query performance degrades.

`OPTIMIZE` merges these small files into larger ones.
## Syntax

```
OPTIMIZE sales;
```

## What happens?

Before:
```
5000 files × 5 MB
```
After:
```
50 files × 500 MB
```