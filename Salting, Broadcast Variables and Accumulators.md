
# Salting for skewness

**Salting** is a technique used in Spark (and distributed systems) to **fix data skew**, especially during **joins or aggregations**. 


![[Pasted image 20260407122413.png|182]] Salting partitions the already partition rows even further. GroupBy is then performed considering the original column and the salt.
- we can decided how much salt we want.(1-3)

## how to salt
1. introduce a new col "salt", use rand() function to fill the columns randomly with valuse between 1-3
2. then group by with a combination of the original column and the salt column 

## Salting in Joins

![[Pasted image 20260407131447.png|369]]

When dealing with Salting in Joins we assign the entire range of salt values as a list to the smaller table begin joined to the larger table. We then explode the smaller table based on the salt list, then join with the larger table.

# SQL Hints in Spark 

Hints are placed after the **Select** statement

```
SELECT
/*+ BROADCAST(df2) */ --HINT
*
FROM 
df1 JOIN df2 
ON df1.id = df2.id AND df1.salt_col = df2.salt_col
```

### BROADCAST Variable 
Broadcast - Making something available in all the executors 

BV - A READ-ONLY Shared variable that is broadcasted to all the executor nodes

```
mp_map ={
    '1': "edible",
    '2' : "lifestyle",
    '3' : "Nature"
}
spark.sparkContext.broadcast(mp_map)
map_value = spark.sparkContext.broadcast(mp_map)
map_value.value
```
no need to create the variable map_value, we fetch it from spark.contex

## Accumulators 

These are WRITE-ONLY shared variables that are used for aggregation.
Executors can add/update the values
Driver will read the value 

- mainly used for tracking bad records
