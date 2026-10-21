Unity Catalog/Unity Catalog Metastore is Databricks' centralized data governance and access-control system.

For example, in your healthcare analytics environment, Unity Catalog can control which users can query Patient 360 tables, which users can access claims data, and who is allowed to create or modify tables.

![[Pasted image 20261009064209.png|340]]

Tables are names using 3-level namespace = ```
com_edp_prd.cmpa_insights_internal_schema.payer360_master```

Purposes:
- Access Control
- Data Discovery
- Data Lineage
- Auditing 
- Governance of AI tools
# Managed vs External Tables

![[Pasted image 20261010070559.png|259]]

- Everything Managed is stored in Databricks itself in one cloud location(MANAGED by Databricks Themselves).
- Everything External is stored in a separate Cloud locations(Managed by US). 

In short: the metastore's metadata is managed by Databricks, while the S3 location configured for Unity Catalog provides cloud storage for data and related artifacts. The two are connected, but they are not the same storage location.

# Volumes

Volumes are objects used to govern access to non-tabular files stored in cloud storage. They let you organize, access, and manage files such as CSVs, JSONs, PDFs, images, and configuration files using Unity Catalog permissions.

Volumes are defined for each particular schema.

![[Pasted image 20261015065234.png]]

We have:
1. Managed Volumes
2. External Volumes

For managed Volumes
`CREATE VOLUME holytestcat.bronze.bronze_volume1`

For External Volume
```
CREATE EXTERNAL VOLUME holytestcat.bronze.bronze_volume1
PATH 'S3/ADLS Path'
```

To query a volume file we can just 
`SELECT * FROM csv.`/Volumes/holytestcat/bronze/bronze_volume/sales/fact_sales.csv``
`SELECT * FROM csv.`PATH`

# DBUTILS

`dbutils` stands for Databricks Utilities. It is a collection of utilities provided by Databricks that lets you perform common tasks from a notebook, such as working with files, managing secrets, and interacting with notebook workflows.

| Utility            | Purpose                                             | Example                                    |
| ------------------ | --------------------------------------------------- | ------------------------------------------ |
| `dbutils.fs`       | Work with files and directories                     | List files, copy files, delete files       |
| `dbutils.secrets`  | Access stored secrets securely                      | Retrieve a database password               |
| `dbutils.widgets`  | Create notebook input parameters                    | Pass a table name to a notebook            |
| `dbutils.notebook` | Run or exit notebooks                               | Call another notebook                      |
| `dbutils.jobs`     | Interact with job context in supported environments | Retrieve information about the current job |

Example:
`dbutils.fs.mkdir('path/foldername')` - creates folder in location
`dbutils.fs.cp('path1','path2')` - changes path to a file