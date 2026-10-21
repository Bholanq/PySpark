#### Q. What is Lakehouse? 
Data Lake + Warehouse

#### Q. Data Lake v/s Data Warehouse v/s Data Marts?
A **Data Lake** is a place where an organization can store **large amounts of data in its original/raw form**.

Structured, Semi-structured, Unstructured, Historical etc

The key idea:
> **"Store the data now; decide how to use it later."**

A **Data Warehouse** is subsections of Data Lake contains cleaned, structured, organized data intended primarily for **analytics**.

A **Data Mart** is a **smaller, focused subset of analytical data**, usually designed for a particular business team or subject area.

# Delta Lake

## What is Open Table Format
An **Open Table Format (OTF)** is a specification that defines **how a table's data and metadata are stored in a data lake** so that different data-processing engines can understand and work with that table.

- Delta Lake
- Apache Iceberg
- Apache Hudi

**Delta Lake is an open table format and storage layer for data lakes.**
Table which are stored using the Delta Lake OTF are called **Delta Tables**.

> **Delta Lake uses Parquet files for the actual data and adds a transaction log `_delta_log` to manage them as a table.**


```
Delta Table
│
├── data files
│
└── _delta_log/
       │
       ├── version 0
       ├── version 1
       ├── version 2
       └── ...
```

# Databricks Architechture


```
                    DATABRICKS
                        │
          ┌─────────────┴─────────────┐
          │                           │
     CONTROL PLANE                DATA/COMPUTE PLANE
          │                           │
   "Manage & control"            "Do the work"
	          │                           │
	   ┌──────┼──────┐             ┌──────┼──────┐
	   │      │      │             │      │      │
	 Notebooks Jobs  UI          Cluster  SQL   Compute
	 APIs    Workflows            Spark  Warehouse
		                                 │
		                                 ▼
		                          Process your data
		                                 │
		                                 ▼
		                          S3 / ADLS / Storage
```

# Classic vs Serverless Compute

Classic - The compute resources run on you cloud Account. You set the capability.
Serverless - Databricks manages the underlying compute infrastructure for you. Autoscaling 

# Computes?

1. All Purpose
2. Job Compute
3. Pool - Faster compute start up time
4. SQL Warehouse 
5. Serverless

The computes except Serverless is Cloud Compute. ie Classic compute.
SQL Warehouse Compute has both classic and Serverless versions
Serverless compute can handle All Purpose, Job and SQL Warehouse

![[Pasted image 20261009062054.png]]