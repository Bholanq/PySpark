- dynamically coalesce the partitions 
- dynamically optimizes the skewness
- dynamically improves the join strategy 

## coalesce
with aqe, only the required amount of partitions and respective tasks will be created + coalesce 
Without aqe a default of 200 partitions are created.
![[Pasted image 20260325160158.png|429]]

## aqe split partitions/ skewness
eg. id 1 had 80% of the data
aqe will break down the skewed partition into smaller partitions.

General rule: if any partition is 5x the median then breakdown + atleast 258 MB

## join strategy 

AQE can change the join strategy between merge-sort, broadcast and shuffle hash during **runtime** based on the table size.







