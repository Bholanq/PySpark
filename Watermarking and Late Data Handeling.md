![[Pasted image 20260701074559.png|537]]

```
.withWatermark("event_time", "10 minutes")
```
This **does not** mean Spark waits 10 minutes after every event.

Instead, Spark tracks the **maximum event time it has seen**.

Suppose Spark has processed events with event times:

```
10:00
10:04
10:07
10:09
10:15 - Latest Event Time 
```

The latest (maximum) event time is:

```
10:15
```

Watermark becomes:

```
10:15 - 10 minutes= 10:05
```

```
Latest Event Time
	10:15
	↓
Watermark10:05
```

Any event older than **10:05** is considered too late.



```
Watermark = Maximum event time seen − Allowed lateness
```


