- You can use all of the sql functions as-it-is in SparkSQL as well
Start with **%sql**

## Spark SQL Scripting Functions BEGIN ->END 

1. FOR 
	1. ```
	   BEGIN 
		   
		END;
		
	   ```
2. IF
3. CASE WHEN THEN 
4. WHILE 

## SparkSQL Auxiliary Statements

1. **DESCRIBE** - gives information about anything we specifiy 
	1. DESCRIBE TABLE table_name - describes the table(partitions, keys, etc)
	2. DESCRIBE QUERY (query) - describes the query
2. **REFRESH** - refreshes the table - basically updates the table in case of any dependencies
	1. REFRESH TABLE table_name 
3. **SHOW** - 
	1. SHOW DATABASES - show the database we're currently in 
	2. SHOW TABLES FROM path - Shows all the tables in the location 
	3. SHOW TABLE EXTENDED LIKE "path%" - specific names
	4. SHOW TBLPROPERTIES tbl_name
	5. SHOW PARTITIONS tbl_name

## SparkSQL aggregate/Adv Functions

### 1. array_agg
array_agg(col) selects all the the product_id entries corresponding to the "order_id"
and puts them in an array.
```
%sql
SELECT customer_id, array_agg(order_id) FROM sql_tbl GROUP BY customer_id
```
eg:
![[Pasted image 20260324121146.png|331]]

Same thing we can do in spark using:
```
df_agg = df_csv.groupBy("customer_id").agg(array_agg("order_id"))
display(df_agg)
```

### 2. collect_list() vs. collect_set()
![[Pasted image 20260324121920.png]]

```
%sql
SELECT customer_id, collect_list(product_id), collect_set(product_id)
FROM sql_tbl
GROUP BY customer_id
```

collect_list() - keeps duplicates while collect_set() does not.

### 3. Statistical functions
	1. corr(col1,col2) - gives the corelation between two columns 
	2. avg(), max() , min(), mode(), median() etc.


## Window Function 

1. LEAD and LAG 
	1. ```
	   %sql
select
order_id,
LAG(order_date,1,'1900-01-01') OVER(order by order_id) as prev_order,
order_date,
LEAD(order_date,1,'9999-01-01') OVER(order by order_id) as next_order
from
sql_tb
	   ```

2. ntile(x) - where x is the max percentile 
This will divide all records into 100 groups based on order_id 
```
%sql
SELECT
order_id,
customer_id,
ntile(100) OVER (ORDER BY order_id) AS order_group
from
sql_tbl
```

This will divide all "cancelled" and "shipped" orders into 100 groups
```
%sql
SELECT
order_id,
order_status,
customer_id,
ntile(100) OVER (PARTITION BY order_status ORDER BY order_id) AS order_group
from
sql_tbl
```

## SQL array functions
array(1,2,3,4)
array_append(arrat(1,2,3),9) - (1,2,3,9)
array_distinct()
array_compact() - removes all nulls
array_contains() - T/F
array_insert( array(),posn, element to be inserted) - at specific position

prepend() - add element at the beginning
array_positon(array(),element) - returns the position of the element in the array. like .find() in py 


