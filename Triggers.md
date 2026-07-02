All triggers except **once** used micro batches.

1. Default Trigger(Real -Time Streaming) - continuously creates and processes batches and wont stop until until all the data is processed (old + new incoming data)
	1. `df.writeStream.start()`
2. processingTime - tries to process all the data available in the scheduled time frame, and then runs again with the newly arrived data.
```
10 records at 5 sec
50 records at 15 sec
100 records at 40 sec
25 records at 100 sec
```
```
0-30 sec
---------
Processes 60 records

30-60 sec
----------
Processes 100 records

60-90 sec
----------
Nothing new
Batch is empty

90-120 sec
-----------
Processes 25 records
```

# What if the previous batch isn't finished processing?
**Spark never runs two micro-batches of the same streaming query in parallel.** If the previous batch hasn't finished, the next scheduled trigger waits.

Let's look at an example.

Suppose you configure:

```
.trigger(processingTime="10 seconds")
```

and each batch takes **25 seconds** to process.

### Timeline

![[Pasted image 20260630100126.png|416]]

Even though the trigger interval is **10 seconds**, Batch 2 **does not start at 10 seconds** because Batch 1 is still running.

Instead:

- Trigger scheduled at 10 sec → skipped (Batch 1 still running)
- Trigger scheduled at 20 sec → skipped
- Batch 1 finishes at 25 sec
- Spark immediately starts Batch 2 at 25 sec

There is **no overlap** between batches.

1. Triggeronce - processes all the available data at once in one batch.
2. availableNow - processes all available data (till when this command is executed) in multiple smaller micro batches then stops.
![[Pasted image 20260630095110.png]]

| Feature                     | Default Trigger      | AvailableNow                                     |
| --------------------------- | -------------------- | ------------------------------------------------ |
| Processes existing data     | ✅ Yes                | ✅ Yes                                            |
| Processes new incoming data | ✅ Yes, continuously  | ✅ Yes, but only while the query is still running |
| Stops automatically         | ❌ No                 | ✅ Yes                                            |
| Runs forever                | ✅ Yes                | ❌ No                                             |
| Uses multiple micro-batches | ✅ Yes                | ✅ Yes                                            |
| Best for                    | Continuous streaming | Incremental ETL / scheduled jobs                 |