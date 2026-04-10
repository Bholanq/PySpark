Modes:
1. Overwrite - If the file doesn't already exist it will create the file, else it will replace the original file.
		```
	df_csv.write.format("csv")\
        .mode("overwrite")\
        .option("path","/Volumes/sparkcatalog/raw/source/destination/csv_overwrite")\
        .save()
		```
2. Append - mode("append") - creates a new file, if it already exists 
3. Error - mode("error") - throws an error if the file already exists
4. Ignore - mode("ignore") - does nothing if the file already exists

# File Formats 

- We can convert aa data frame which is one format and then store them in any format(csv, json, parquet) using the .save() command. 
- eg.
- ```
  df_csv.write.format('json')\
    .mode("overwrite")\
        .option("path","/Volumes/my_catalog/raw/source/destination/json")\
        .save()
  ```