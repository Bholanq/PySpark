Delta Live Table
**Delta Live Tables (DLT)** (now commonly referred to in Databricks as **Lakeflow Declarative Pipelines**) is a managed framework for building **reliable ETL/ELT data pipelines** on Databricks.

Instead of writing code to manually orchestrate reads, writes, dependencies, retries, checkpoints, and data quality checks, you simply **declare what each table should contain**, and Databricks manages the pipeline execution.

![[Pasted image 20260701083701.png]]

# Traditional Spark vs DLT

### Traditional Spark

```
bronze = (    spark.readStream         .format("cloudFiles")         .load("/landing"))bronze.writeStream.table("bronze")
```

Then:

```
silver = spark.read.table("bronze")
```

Then:

```
silver.write.saveAsTable("silver")
```

You manually control every step.
### DLT

```
from pyspark import pipelines as dp
 @dp.tabledef bronze():    
	 return (
	         spark.readStream
	                      .format("cloudFiles")             
	                      .load("/landing")   
	        )
 @dp.tabledef silver():
		     return spark.read.table("bronze")
```

You simply define the transformations.

DLT figures out:

- Execution order
- Dependencies
- Table creation
- Incremental processing
- Recovery after failures
![[Pasted image 20260701084402.png]]