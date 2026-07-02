Array Functions
![[Pasted image 20260617125508.png|323]]

#### Explode and LATERAL VIEW

| employee | skills             |
| -------- | ------------------ |
| John     | [SQL,Python,Spark] |
| Alice    | [AWS,Spark]        |

```
SELECT
    employee,
    powers
FROM employees
LATERAL VIEW explode(powers) s AS powers;
```

![[Pasted image 20260617134523.png|363]]
We can use explode directly in SELECT statement, but this creates a problem when there's multiple columns to explode. Spark doesn't know what should be the spine.

In case of multiple columns that need to be exploded:
# Why can't I write `explode(col1), explode(col2)`?

**Spark SQL allows only one generator function (such as `explode`, `posexplode`, `inline`) directly in a `SELECT` clause** because each generator can produce multiple output rows. Having two generators in the same projection is ambiguous.

Instead, you either:

- **Zip related arrays** using `arrays_zip()` and explode once, or
- **Explode sequentially** using nested queries or `LATERAL VIEW`.

`array_zip` - combines two columns, even if they're lists. In case of lists all possible combinations are created.

## Nesting 

|id|colors|sizes|
|---|---|---|
|1|[Red, Blue]|[S, M]|
```
SELECT
    id,
    color,
    explode(sizes) AS size
FROM (
    SELECT
        id,
        explode(colors) AS color,
        sizes
    FROM products
);
```

## Lateral View

```
SELECT
    id,
    color,
    size
FROM products
LATERAL VIEW explode(colors) c AS color
LATERAL VIEW explode(sizes) s AS size;
```

| id  | color | size |
| --- | ----- | ---- |
| 1   | Red   | S    |
| 1   | Red   | M    |
| 1   | Blue  | S    |
| 1   | Blue  | M    |
Deduplication and Validation Logic
![[Pasted image 20260617132319.png|380]]

Timestamp and String Function
![[Pasted image 20260617134751.png|548]]

Regex Pattern
![[Pasted image 20260617161529.png|407]]

```
REGEXP_EXTRACT()
```

DOT_SYNTAX
![[Pasted image 20260617163537.png|315]]

COUNT vs COUNT_IF
![[Pasted image 20260617163753.png|320]]

[[CACHE STATEMENTS]]
![[Pasted image 20260617164449.png|348]]