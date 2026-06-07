| System                   | Storage Style | Typical Workload |
| ------------------------ | ------------- | ---------------- |
| PostgreSQL               | Row           | OLTP             |
| MySQL                    | Row           | OLTP             |
| Oracle                   | Row           | OLTP             |
| SQL Server               | Row           | OLTP             |
| Databricks Delta/Parquet | Columnar      | OLAP             |
| Snowflake                | Columnar      | OLAP             |
| Redshift                 | Columnar      | OLAP             |
| BigQuery                 | Columnar      | OLAP             |
| ClickHouse               | Columnar      | OLAP             |

![[Pasted image 20260606054544.png|450]]

| Aspect                | OLTP (Online Transaction Processing) | OLAP (Online Analytical Processing)   |
| --------------------- | ------------------------------------ | ------------------------------------- |
| Purpose               | Run business operations              | Analyze business data                 |
| Users                 | Applications, customers, employees   | Analysts, data scientists, executives |
| Query Type            | Small reads/writes                   | Large aggregations/scans              |
| Data Volume per Query | Few rows                             | Millions/billions of rows             |
| Response Time         | Milliseconds                         | Seconds to minutes                    |
| Updates               | Frequent                             | Infrequent                            |
| Storage Preference    | Row-oriented                         | Column-oriented                       |


#### OLTP systems power day-to-day business operations.
**Updates/Inserts/Deletions**
Examples:
- Placing an order on an e-commerce website
- Withdrawing money from a bank account
- Updating a customer profile
- Booking a flight ticket

A typical OLTP query:

```
SELECT *FROM customersWHERE customer_id = 123;
```
Or:
```
UPDATE accountsSET balance = balance - 100WHERE account_id = 1001;
```
Characteristics:
- Accesses a small number of rows
- Requires fast response times
- Many concurrent users
- Frequent INSERT, UPDATE, DELETE operations
- Strong ACID guarantees

#### OLAP systems answer business questions using large amounts of historical data.
**Aggregates etc**
Examples:

- What were sales by region last quarter?
- Which products grew fastest this year?
- Which customers are likely to churn?
- How effective was a marketing campaign?

Typical OLAP query:

```
SELECT    region,    SUM(sales)FROM sales_factGROUP BY region;
```

This may scan billions of rows.

Characteristics:

- Reads huge datasets
- Mostly read-only
- Complex aggregations
- Joins across many tables
- Historical analysis