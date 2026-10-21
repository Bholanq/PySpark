Questions:
1. When do you say that a record is corrupted?
2. What happens when you encounter a corrupted record in different read mode?
3. How can we print different corrupted records
4. Where can you store them, and how can you access it later?

#### 1. 
In the case of a JSON a missing bracket could indicate a bad corrupted record
: {key:value},{key,value

In the case of CSV, it would be something like missing column entries or extra entries in each row:

id,name,age,salary,address,nominee 
1,Manish,26,75000,bihar,nominee1 
2,Nikita,23,100000,uttarpradesh,nominee2 
3,Pritam,22,150000,Bangalore,India,nominee3 
4,Prantosh,17,200000,Kolkata,India,nominee4 
5,Vikash,31,300000,,nominee5

#### 2.  
For the above CSV
`FAILFAST` - the process stops when any row has more than expected entries, the null in the 5th columns is not malformed 
`DROPMALFORMED` - Drops only the rows with extra entries the NULLs are kept
`PERMISSIVE` - Passes all records in this case, instead of nominee3 and 4 it considers "india" under the nominee column.

#### 3. 
We can print corrupted records using a `_corrupt_record"` and adding that to that schema
```
emp_schema = StructType([
StructField("id",Integertype(),True),
....
StructField("_corrupt_record",StringType(),True)
])
```

```
df =spark.read.format("CSV")\
.option()\
.schema(emp_schema)\
.path()

df.show(TRUNCATE = FALSE)
```

#### 4. 

`.option("BadRecordPath","<PATH>")\`

will store the bad records in <PATH>