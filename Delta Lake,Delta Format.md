Delta Lake/ Delta Format - Parquet File + Delta_log(meta_data)

Parquet file alone doesn't have SQL Table properties:
- ACID properties
- Optimization/Indexing
- Commit & Rollback
- Structured Query

![[Pasted image 20260323140843.png|381]]

part-00000.. is the actual parquet file.
delta_log as the name suggests is the file containing all the meta data about the parquet file.
`_delta_log`- enables the following properties:

![[Pasted image 20260323141136.png|303]]
