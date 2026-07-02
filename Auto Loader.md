![[Pasted image 20260701080451.png]]

Auto Loader keeps track of the files and only ingests the new files into databricks.

![[Pasted image 20260701080620.png]]

# How does Auto Loader know what it already processed?

It uses a **checkpoint**.

```
df.writeStream \
		  .option("checkpointLocation", "/checkpoints/orders")
```

The checkpoint stores metadata such as:

- Processed file names (or identifiers)
- Progress information
- Streaming offsets
- Schema evolution metadata (when applicable)

When the stream restarts, it resumes from the checkpoint instead of starting over.

File Detection Modes:
# Directory Listing vs File Notification

![[Pasted image 20260701081034.png]]

# Schema Inference

Suppose the first file is:

```
id,name1,Alice2,Bob
```

Auto Loader infers:

|Column|Type|
|---|---|
|id|Integer|
|name|String|

No schema definition is required for many simple use cases, though providing an explicit schema is often recommended in production.

---

# Schema Evolution

Suppose tomorrow a new file arrives:

```
id,name,city3,Charlie,London
```

The schema has changed.

Auto Loader can detect this and evolve the schema when configured.

Example:

```
.option("cloudFiles.schemaEvolutionMode", "addNewColumns")
```

Now the table becomes:

|id|name|city|
|---|---|---|
|1|Alice|NULL|
|2|Bob|NULL|
|3|Charlie|London|

Existing rows simply have `NULL` for the new column.

By **default, Auto Loader behaves differently depending on the file format**. The statement that "Auto Loader infers everything as `STRING`" is **true for text-based formats like CSV, JSON, and XML**, but **not for self-describing formats like Parquet and Avro**.

Here's the breakdown.

|File Format|Default Schema Inference|
|---|---|
|CSV|**All columns as `STRING`** by default|
|JSON|**All fields as `STRING`** by default|
|XML|**All fields as `STRING`** by default|
|Parquet|Uses the types stored in the Parquet file|
|Avro|Uses the types stored in the Avro schema|
|ORC|Uses the types stored in the ORC file|
