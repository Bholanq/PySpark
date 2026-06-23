- **CACHE TABLE** - as the name suggests puts a table into cache
```
CACHE TABLE table_name
```

- **CLEAR CACHE** - clears the cache of ALL tables in the cache

`CLEAR CACHE table_name`

- **UNCACHE TABLE** - uncaches that particular table

`UNCACHE TABLE table_name`

- **REFRESH CACHE** - Refresh metadata and invalidate cached data for a table

`REFRESH TABLE sales`

If there are any changes made in the original table in the disk, that are not in the cache table. This command checks that, and if any discrepancy is found in the cache vs disk, then that cached table is marked as invalid.

the cached table is re-written only when we once again use that table.


- **REFRESH CACHE** - refresh all cached entries that depend on changed data
`REFRESH CACHE`


- **REFRESH FUNCTION** - refreshes cached function, the function isn't re-written unless its called again.
	Used for UDFs.

- 