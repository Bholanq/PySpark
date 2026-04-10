### Splitting and Indexing 
splitting - creating multiple columns using a single column - will result in a list - select from that list 
- works in conjunction with split column 

`df.withColumn("new_col_name",split("og_col",'splitting_condn'))`

`df_split = df_csv.withColumn("street_address",split('shipping_address',',')[0])\
``		         .withColumn("city_name",split('shipping_address',',')[1])

### Explode
Explodes an array, ie. Creates separate rows for each element of the array.
- can only be performed on a column with arrays

-works in conjunction with withColumn
1. create array column using split 
	1. `df_arr = df_csv.withColumn("address_array",split("shipping_address",","))

2. explode/explode_outer
	1. df_exp = df_arr.withColumn("address_explode",explode("address_array"))
	2. df_exp = df_arr.withColumn("address_explode",explode_outer("address_array"))

## Array Contains
	
- returns bool
`df_cont = df_csv.withColumn("new_col",arrayContains("og_col","target"))

## GroupBy

	-used outside of withColumn 

`df_agg = df_csv.withColumn("total_price",col('price')*col('quantity'))\
            .groupBy('product_id').agg(sum('total_price').alias('total_price'))\
            .withColumn("total_price",round(col('total_price'),2))\
            .sort(col('total_price').desc())

		APPROX_COUNT_DISTINCT
			-Gives approximate count
	

### Collect list - creates a list of all records corresponding to some other record 

`df_collect = df_csv.groupBy('customer_id').agg(collect_list('product_id').alias('products'))

## PIVOTING 

`df_pivot = df_csv.groupBy('customer_id').pivot('order_status').agg(round(count('order_id'),2))`

 `groupby("to_be_row_column").pivot("to_be_col_column").agg()
 `
## WHEN OTHERWISE - IFELSE 

`df_ifelse = df_pivot.withColumn('return_flag',when(col('Returned')==1,'low').when(col('Returned')<=3,'mid').when(col('Returned')>3,'high').otherwise('no returns'))


## Joins in spark

1. Inner Join
		df_inner = df1.join(df2,df1['id'] == df2['id'],'inner')
2. left 
	1. 		df_inner = df1.join(df2,df1['id'] == df2['id'],'left')
3. 'full'
4. 'anti', left exclusive 
	1. ![[Pasted image 20260319162726.png|261]]
## Window Functions
`df_ranking = df.withColumn("rank",rank().over(Window.orderBy('marks')))\
  ``              .withColumn("dense_rank",dense_rank().over(Window.orderBy('marks')))

## Cumulative 
df_cum = df.withColumn('cumulative', sum().over(Window.OrderBy("Year")).rowsBetween(Window.unboundedpreceding,window.currentrow))


## Concat transformations

Concat and ConcatWS

concat is used to join the data from two different columns where we specify each separator ourselves and concat_ws does the same but we specify the separator beforehand  


df_con = df_csv.withColumn("custom_id",**concat**(col('customer_id'),lit('|'),col('order_id'),lit('|'),col('product_id')))
display(df_con)


df_con = df_csv.withColumn("custom_id",**concat_ws**(lit('|'),col('customer_id'),col('order_id'),col('product_id')))
display(df_con)


