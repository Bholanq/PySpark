Spark connect introduced de-coupled client server architecture. 

GRPC Protocol

Decoupled client-server architecture that allows remote connectivity to Spark clusters using the DataFrame API and unresolved logical plans as the protocol. The separation between client and server allows Spark and its open ecosystem to be leveraged from everywhere. It can be embedded in modern data applications, in IDEs, Notebooks and programming languages.

![[Pasted image 20260403130022.png|399]]

![[Pasted image 20260403130219.png|383]]

### using apache spark 
```
from pyspark.sql import SparkSession
spark_new = SparkSession.builder.appName("SparkByExamples.com")\
.remote("spark://spark-master:7077")\
.getOrCreate()
```

### using databricks - databricks connect 

![[Pasted image 20260403131030.png|443]]


