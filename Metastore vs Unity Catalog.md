**Hive Metastore** and **Unity Catalog** both manage metadata (tables, schemas, databases), but **Unity Catalog is a complete governance layer** built on top of the metastore concept. Think of it this way:

- **Hive Metastore = a catalog of metadata**
- **Unity Catalog = a centralized governance platform that includes a metastore plus security, lineage, sharing, and more**

| Feature                 | Hive Metastore                       | Unity Catalog                                              |
| ----------------------- | ------------------------------------ | ---------------------------------------------------------- |
| Primary purpose         | Store metadata for Spark/Hive tables | Centralized governance for all data assets                 |
| Metadata storage        | Tables, databases, columns           | Tables, views, volumes, models, functions, AI assets, etc. |
| Access control          | Table/database level (limited)       | Fine-grained (catalog, schema, table, column, row)         |
| **Multiple workspaces** | **Difficult**                        | **Designed for multiple workspaces**                       |
| [[Data lineage]]        | No                                   | Built-in                                                   |
| Data sharing            | No                                   | Supports Delta Sharing                                     |
| Auditing                | Limited                              | Comprehensive audit logs                                   |
| Credential management   | Manual                               | Centralized storage credentials                            |
| Cloud support           | Mostly workspace-specific            | Cross-cloud governance                                     |
| **Recommended**         | **Legacy**                           | **Current Databricks standard**                            |


# Unity Catalog 
- 3 level namespace
- catalog.schema.table
![[Pasted image 20260702070040.png]]

[[UC Tables]]
[[UC Volumes]]
[[UC Views]]

