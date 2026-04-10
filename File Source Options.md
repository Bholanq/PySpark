### 1. Ignore corrupt files

tells Spark to **skip corrupted or unreadable files instead of failing the entire job**
```
test_corrupt_df0 = spark.read.option("ignoreCorruptFiles", "true")\ .parquet("examples/src/main/resources/dir1/", "examples/src/main/resources/dir1/dir2/"
```
### 2. ignore missing files
continue even if some input files are missing

### 3. PathGlobFilter
lets you **read only specific files from a directory using wildcard patterns (glob patterns)**.
Instead of reading all files in a directory, you can filter:
- By file name pattern
- By extension
- By prefix/suffix

df = spark.read \
    .option("pathGlobFilter", "*.csv") \
    .load("/mnt/data/")
- reads all csv files 

df = spark.read \
    .option("pathGlobFilter", "sales_*.parquet") \
    .parquet("/mnt/data/")
- reads files with specific patterns 
### 4. recursiveFileLookup - scan all subdirectories

	recursive_loaded_df = spark.read.format("parquet")\ .option("recursiveFileLookup", "true")\ .load("examples/src/main/resources/dir1")
### 5. modifiedBefore & modifiedAfter

```
df = spark.read.load("examples/src/main/resources/dir1", format="parquet", modifiedBefore="2050-07 01T08:30:00") df = spark.read.load("examples/src/main/resources/dir1", format="parquet", modifiedAfter="2050-06 01T08:30:00")
```
