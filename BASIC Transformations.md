## Select
`df_select = df_csv.select(col('customer_id'),col("order_id"),col("order_date"),col("product_id"),col("quantity"))
## Alias
`df_alias = df_csv.select(col("customer_id"),col('city').alias('customer_city'))
## Filter
`from pyspark.sql.functions import col
`df = df_csv.filter(col('returned') == 'Yes')

multiple filters:

`df = df_csv.filter((col('returned') == 'Yes') | (col('returned') == 'Cancelled'))
`.filter(col("order_status") isin("","",""))

## withColumnRenamed - Changes the column name
df_rewnamed = df_csv.withColumnRenamed('order_id','order_id_rewnamed')

### WithColumn - tells sparks we're either modifying a column or creating a new one

**Scenario1**: Creating a new col with a default val

`df_mod1 = df_csv.withColumn('flag',lit(0))
display(df_mod1)


Scenario2: replacing a part of a string  
![[Pasted image 20260317170556.png|194]]

eg. want to remove city from shipping address
`df_mod2 = df_csv.withColumn('shipping_address',regexp_replace('shipping_address',',.*', ''))

scenario3: Creating a total column and then rounding them off(stacking multiple transformations)

`df_mod3 = df_cvs.withColumn('Total' ,col('quantity')* col('price))
``				.withColumn('Total',round(col('Total'),2))

## Type Casting 

`df_cast2 = df_csv.withColumn('order_id',col('order_id').cast('STRING'))
`df_cast1 = df_csv.withColumn('order_id',col('order_id').cast(StringType()))

## Sorting 

	df_sort = df_csv.sort(col('order_date').desc())		

scenario2: sorting multiple columns 
`df_sort2 = df_csv.sort(['order_date','quantity'],ascending = [0,1])`

## Limit 

`df_limit = df_csv.limit(10)`

## Drop

`df_drop = df_csv.drop('col_name','col_names')

## Drop Duplicates

`df_dedupes = df_csv.dropDuplicates()` - this will drop all rows where all the entries are the same.

scenario2: dropping duplicates only from specific columns

`df_dedupes2 = df_csv.dropDuplicates(subset = ['order_date','product_id'])

## Union & UnionbyNames
Union tries to stack columns without checking the column name, whereas unionByName matches column names before performing the union.
Generally, unionbyname is preferred 
`df_union = df1.union(df2)
`df_union2 = df1.unionByName(df2)

## Date Functions
-wayy too many date functions - works in conjunction with withColumn

1. .withColumn('new_column_name',current_timestamp())
2. .withColumn('new_column_name',date_add("og_col_name",1))
3. .withColumn('new_column_name',(date_format"og_col_name",'yy-mm-dd'))


## String functions

-used in conjunction with .withColumn("og_col_name",action_on_col("col_name"))
1. df_str = df_csv.withColumn("order_status_length",length('order_status'))
2. df_str = df_csv.withColumn("order_status",lower('order_status'))

## Handling Nulls

1. drop all nulls
	1. df.dropna('all) - this will drop all rows which are completely null
2. any nulls - will drop the record if any of the column is null
	1. df.dropna('any') 
3. subsets 
	1. how = 'all' - will only drop the records where all the mentioned columns are `nulls
		1. df.dropna(subset = ['name','address'], how = 'all')
	2. how = 'any'
		1. df.dropna(subset = ['name','address'], how = 'any')
4. Fill nulls
	1. df.fillna('dummy') - will only fill nulls in the columns with the matching datatypes
	2. df.fillna('name':'dummy_name', 'address':'dummy_address')























