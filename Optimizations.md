![[Pasted image 20260615161542.png|462]]

[[OPTIMIZE]]
[[ZORDER]]
[[LIQUID CLUSTERING ]]
[[VACCUME]]


The **files** are stored in the **cloud object storage that your Databricks workspace is configured to use**.
### 1. AWS Databricks

The files are typically stored in **Amazon S3**.
For example:
```
s3://company-data-lake/sales/├── _delta_log/├── part-00000-....snappy.parquet├── part-00001-....snappy.parquet└── ...
```
### 2. Azure Databricks
The files are usually stored in **Azure Data Lake Storage Gen2 (ADLS)**.
Example:
```
abfss://gold@companydatalake.dfs.core.windows.net/sales/├── _delta_log/├── part-00000-....snappy.parquet└── ...
```
### 3. Google Cloud Databricks
The files are stored in **Google Cloud Storage (GCS)**.
Example:
```
gs://company-data-lake/sales/├── _delta_log/├── part-00000-....snappy.parquet└── ...
```

# Managed vs. External Tables 

### Managed tables
Example:
```
CREATE TABLE sales (    order_id INT,    amount DOUBLE)USING DELTA;
```
Databricks chooses the storage location automatically.

The files go into the workspace's **managed storage location**.

Historically, it looked something like:

```
dbfs:/user/hive/warehouse/sales/
```

However, **DBFS is not the actual storage**.

It is just a **mount/interface** over the underlying cloud storage.

For example:
```
dbfs:/user/hive/warehouse/sales/
```
might physically correspond to:
```
s3://databricks-workspace-storage/.../sales/
```
or
```
abfss://workspace-container@storageaccount.dfs.core.windows.net/.../sales/
```
depending on the cloud.
### External tables
You specify exactly where the data lives.
Example:
```
CREATE TABLE salesUSING DELTALOCATION 'abfss://gold@companylake.dfs.core.windows.net/sales';
```
Now Databricks stores metadata about the table, but the actual files remain in:
```
abfss://gold@companylake.dfs.core.windows.net/sales/
```

**Managed vs external is not about whether the data is in the cloud.**
It's about **who is responsible for managing the storage lifecycle** of that cloud data:

- **Managed table → Databricks manages**
- **External table → You manage it.**

# Biggest Difference - DROP TABLE

## Managed table

```
DROP TABLE sales;
```

Result:

```
Metadata removed.Parquet files deleted.Delta log deleted.
```
Everything disappears.
## External table
```
DROP TABLE sales;
```
Result:
```
Metadata removed.Parquet files remain.Delta log remains.
```
The data still exists.