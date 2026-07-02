![[Pasted image 20260618090105.png|379]]
# Reading data from external sources

![[Pasted image 20260618115714.png|484]]
- We can use the `OPTIONS` to use CSVs in cloud storage 
					  OR
- Unlike in the image above, JDBC is (Java Database Connection) is specifically for Database Servers not for files in cloud storage. 

# Generated Columns
![[Pasted image 20260618153823.png|489]]

# Table Comments
```
spark.sql("""
CREATE TABLE IF NOT EXISTS anime_characters (
    character_id STRING,
    name STRING,
    anime STRING,
    power_level INT
)
COMMENT 'Table of anime characters with their power levels, baka!'
""")
```

[[DML]]

# [[DESCRIBE STATEMENTS]]
![[Pasted image 20260618160505.png|435]]


# UDFs
![[Pasted image 20260618165259.png|612]]
`CREATE FUNCTION ...`

EXAMPLE 2:
![[Pasted image 20260630093846.png]]

# File Format Prefix

![[Pasted image 20260618180717.png]]

[[File Handling]]
