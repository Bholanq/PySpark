https://www.youtube.com/watch?v=yq_gOzSQQY4.
Tumbling window - 
```
from pyspark.sql.functions import window, count
df_str = spark.readStream.table('sparkcatalog.raw.stream_source')\
    .groupBy("color",window('event_time','10 minutes')).agg(count('color').alias("count"))
```
Sliding Window - 
```
from pyspark.sql.functions import window, count
df_str = spark.readStream.table('sparkcatalog.raw.stream_source')\
    .groupBy("color",window('event_time','10 minutes','5 minutes')).agg(count('color').alias("count"))
```

if we keep the third parameter in **window**  = '0 minutes', then it is the same a tumbling window.


Session window - dynamic sized windows - the window only activates when a event occurs and stays alive for the gap durations
![[Pasted image 20260330170314.png|433]]

Watermark - 
eg. 10 min 
![[Pasted image 20260330165601.png|627]]

data thats processed at 12:04 but arrives b/w 12:15 and 12:20 is considered to be an late arrival as it arrives in the next window. 

if the watermark is 10 min then only the data arriving at or after
`max(arrival time in that window) - watermark
eg. 12:17-10 = 12:07
then it is still considered for the window it was originally meant for else discarded
so that data processed at 12:04 is not considered


```
from pyspark.sql.functions import window, count

df_str = spark.readStream.table('sparkcatalog.raw.stream_source')\

    .groupBy("color",window('event_time','10 minutes')).agg(count('color').alias("count"))
```

This will create a window of event_time -> event_time+10 mins

![[Pasted image 20260402131956.png|583]]

## Late Arrivals of Events 


## Watermarks 

In **PySpark Structured Streaming**, a **watermark** is used to handle **late-arring data** in streaming data processing.

`df.withWatermark("event_time_column", "10 minutes")`

Allowed Event time for late arrivals = Max(event_time upto now) - watermark time 


