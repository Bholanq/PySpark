![[Pasted image 20260306164236.png|333]]
RDDs are the backbone of the Spark Ecosystem 
RDD - Resilient Distributed Dataset
 - Fundamental DS of spark used to store and process large datasets across a cluster.
 - **RDD (Resilient Distributed Dataset)** is an **immutable distributed collection of objects** that can be processed **in parallel across multiple machines**.

![[Pasted image 20260306165743.png|412]]

Q1. Why do we not directly write our code in rdds? If the code is converted from Dataframes to RDDs anyways?

- RDDs dont have any schemas - cant apply optimizations 
- used to write in RDD - not very readable 
- we had to tell HOW along with WHAT while writing the code

**RDD are not real data they're just logical partitions**
![[Pasted image 20260306181707.png|389]]

