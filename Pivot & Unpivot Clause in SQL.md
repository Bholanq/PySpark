![[Pasted image 20260630090833.png]]

Unique row values into column headers summarizing the values across them

```
SELECT *
FROM table_name
PIVOT (
    aggregate_function(value_column)
    FOR pivot_column IN (value1, value2, ...)
);
```

```
SELECT *
FROM sales
PIVOT (
    SUM(sales)
    FOR month IN ('Jan', 'Feb')
);
```


`UNPIVOT` is the opposite of `PIVOT`.

- **PIVOT** converts **rows → columns**.
- **UNPIVOT** converts **columns → rows**.

```
SELECT *
FROM sales
UNPIVOT (
    sales
    FOR month IN (Jan, Feb, Mar)
);
```

