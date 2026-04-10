Pandas generally follow the concept of vertical scaling (ie one machine), not ideal for very large datasets.
Implementing Pandas with Spark allows for horizontal scaling. 
![[Pasted image 20260403140339.png|358]]


Adv:
1. Same pandas syntax 
2. Handles big data 
3. Batter performance of python UDFs

## Some basic functions

```
import pyspark.pandas as ps

#reading
df = ps.read_csv('/Volumes/sparkcatalog/raw/source/raw_orders/orders.csv')
df.head()

df.describe()

#selecting 
df[['order_id']].head(5)

# creating new cols
df['new_col'] = df['price']*10
df['flag'] = df['price']>1000
df.head()

#filtering 
df[df['price']>50].head()

#sorting 
df.sort_values('price', ascending=False).head()
```


## Pandas UDFs are Better??

- **Pandas UDFs are faster and more efficient than regular Python UDFs** in PySpark because they reduce overhead and use vectorized operations.

![[Pasted image 20260403160805.png|615]]

### Python UDF

- Operates **row-by-row**
- Each row is sent from JVM (Spark) → Python → back to JVM
- Heavy **serialization/deserialization overhead**
[[Speed+Arrow vs Series+Typehinting]]

### [[Pandas UDF]]

- series2series
- iterator2iterator
- multipleiterator2iterator 
- series2scalar


