Record_at_a_time -> Spark_Streaming(micro-batch-streaming, D-streaming) -> Spark_structured_streaming

prerequisites:
1. [[Data Writing, Batch Output Modes]]
2. [[File Source Options]]

## Spark Structured Streaming Model

New Streams - New data in the form of records
Unbounded Table - Appendable Table 

**incrementalization** - user defines a batch-line query on top of the unbounded input table, structured streaming will automatically convert the batch like query to structured streaming execution plan

## Stateless vs Stateful Transformations
**Stateless** - When the transformation is independent of the previous data. eg. when using filter transformation

eg. When new streams arrive it does not need to check the data previously processed in order to use the filter transformation. It can simply filter the new stream on the given conditio.

**Stateful** - When the transformation is dependent of the previous data. eg. when using group by
eg2. running totals

## Checkpoint Director 
 - sources
 - offsets
 - commits 
 - metadata
## Output modes

### **Append** - only available for stateless transformations - only insert the new records
1. incase of duplicate records it will write that record into the sink again
2. Append mode **only works with aggregation if you use watermark + window**  
(so Spark knows when results are final).
```
#Enabling schema inference for JSON files
spark.conf.set("spark.sql.streaming.streaming.schemaInference" ,"True")

df_batch = spark.read.format("json")\
			.option("multiline","true")\
			.option("inferschema","true")\
			.load("add")
			
			
json_schema = df_batch.schema
#copy schema form the json read



#stream read
df = spark.readStreamr.format("json")\
	.option("multiline",True)\
	.schema(json_schema)\
	 .option("cleanSource","archive")\
	.option("sourceArchiveDir","/Volumes/sparkcatalog/raw/source/source_archive/")\
        .load("/Volumes/sparkcatalog/raw/source/src_append/")

#stream write
query = spark.writeStream.format("delta")\
		.output("append")\
		.trigger(processingTime="5 seconds")\
			.option("checkpointLocation",  "/Volumes/sparkcatalog/raw/source/sink_append/checkpoint")\
        .option("path", "/Volumes/sparkcatalog/raw/source/sink_append/data")\
        .start()
        
query.stop()

```

### "**Complete**" Output modes 
	It will re-write all rows in the sink every time we trigger it 
	1. only in structured streaming 



In **Structured Streaming**, output modes depend on whether you're doing **aggregation or not**:

| Query Type                              | Allowed Output Modes           |
| --------------------------------------- | ------------------------------ |
| **No aggregation** (just select/filter) | `append` only                  |
| **Aggregation present**                 | `append`, `update`, `complete` |
	1. ![[Pasted image 20260325194044.png|365]]
3. **Update** Output mode 
	1.  works with aggregations
	2. 

## Trigger Modes 

### 1. Default 
It will automatically trigger the next stream as soon as the previous steam is complete.

### 2. processingTime - micro -batches 
We define at what intervals spark needs to trigger the job
```
df.writeStream.format("delta")\
        .outputMode("complete")\
        .trigger(processingTime="5 seconds")\
        .option("checkpointLocation", "/Volumes/sparkcatalog/raw/source/sink_complete/checkpoint")\
        .option("path", "/Volumes/sparkcatalog/raw/source/sink_complete/data")\
        .start()
```
## 3. once
It will load all the data once and then will stop the stream 
- backfilling and initial load

```
  .trigger(once=True)\
```
### 4. availableNow
same as once, but does the load in multiple micro-batches
```
.trigger(availableNow=True)\
```
## continuous - almost real time 

```
df.writeStream.format("delta")\
        .outputMode("complete")\
        .trigger(continuous='1 second')\
        .option("checkpointLocation", "/Volumes/sparkcatalog/raw/source/sink_complete/checkpoint")\
        .option("path", "/Volumes/sparkcatalog/raw/source/sink_complete/data")\
        .start()
```

trigger(processingTime = '5 seconds') - processes the micro-batch every 5 seconds
trigger(continuous = '5 seconds') - stream is continuous but the checkpoint is updated every 5 second

## Archive Source File 
In order to avoid clustering in the Source Folder, after a file is already processed it moved to the archive folder aka Archive Source File.
![[Pasted image 20260401124346.png|333]]

## forEachBatch 

allows you to pass each micro-batch through user's function 
eg. instead of writing the data into only one sink when can make sure each batch into however many sinks we want.

```
df = spark.readStream.table('sparkcatalog.raw.complete_source')

def my_function(df,batch_id):

    # Writing To Stream 1 (Will Have To Use Batch Code)
    df.write.format("parquet")\
            .mode("overwrite")\
            .option("path","/Volumes/sparkcatalog/raw/source/streams/stream1")\
            .save()

    # Writing To Stream 2 (Will Have To Use Batch Code)
    df.write.format("parquet")\
            .mode("overwrite")\
            .option("path","/Volumes/sparkcatalog/raw/source/streams/stream2")\
            .save()

    # Can Use Batch Logic To Apply UPSERT

df.writeStream.foreachBatch(my_function)\
        .outputMode("append")\
        .option("path","/Volumes/sparkcatalog/raw/source/streams/checkpoint")\
        .option("path","/Volumes/sparkcatalog/raw/source/streams/data")\
        .start()
```

-**df.writeStream.foreachBatch(function)**\
		.everything here remains the same 


## Event Time vs Processing Time

Event Time - When the event was actually created ie. when the data was generated 
Processing Time - When the event was processed ie. when the data is ready to be used 

## [[Window Operations + Watermarks + Late Arrivals]] in Streaming 

- different from sql window functions
- Similar to Event Time Timstamp
- tumbling window 
- sliding window 
- session window 
![[Pasted image 20260327164317.png|535]]


