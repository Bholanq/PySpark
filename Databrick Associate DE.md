![[Pasted image 20260606044112.png]]

1. [[Databricks Intelligence Platform]]
2. [[Development and Ingestion]]
3. Data Processing & Transformations
4. Productionizing Data Pipelines
5. Data Governance and Quality 

![[Pasted image 20260606045039.png|572]]

Data - Unit of Information
Data Type - A single unit of data which tells a compiler or interpreter how it's intended to be used. `INT FLOAT STRING etc`

#### Schema(SQL) vs Schemaless(NoSQL) 

![[Pasted image 20260606051306.png|409]]

![[Pasted image 20260606051559.png|413]]

#### Data Document 

![[Pasted image 20260606051746.png|507]]

Datasets - Logical Grouping of data that generally are closely related and/or share the same type of data structure.

[[Pivot Table]] - Summarizes the data of another table.

#### View vs Materialized View
- View - Result set of a stored query on data stored in memory
- Materialized View - Stored in Disk

[[Row vs Columnar Storage]]
### Normalized vs De-normalized data
![[Pasted image 20260606055647.png|449]]

#### Strongly Consistent vs Eventually Consistent data

> **Strong consistency guarantees that every read sees the most recent successful write. Eventual consistency guarantees that all replicas will converge to the same value eventually, but some reads may temporarily see older data.**

![[Pasted image 20260606060519.png]]

# Databricks Repos vs Native Notebook Versioning 
| Feature                               | Native Notebook Versioning | Databricks Repos  |
| ------------------------------------- | -------------------------- | ----------------- |
| Version history                       | Yes                        | Yes (via Git)     |
| Branching                             | No                         | Yes               |
| Pull Requests                         | No                         | Yes               |
| Code Reviews                          | No                         | Yes               |
| Merge changes                         | No                         | Yes               |
| Restore old versions                  | Yes                        | Yes               |
| Collaborate with developers           | Limited                    | Full Git workflow |
| Connect to GitHub/Azure DevOps/GitLab | No                         | Yes               |
