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
    skill
FROM employees
LATERAL VIEW explode(skills) s AS skill;
```

![[Pasted image 20260617134523.png|363]]
We can use explode directly in SELECT statement, but this creates a problem when there's multiple columns to explode. Spark doesn't know what should be the spine.


Deduplication and Validation Logic
![[Pasted image 20260617132319.png|380]]

Timestamp and String Function
![[Pasted image 20260617134751.png|400]]

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