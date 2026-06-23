All triggers except **once** used micro batches.

1. Default Trigger - continuously creates and processes batches and wont stop until until all the data is processed (old + new incoming data)
	1. `df.writeStream.start()`
2. processingTime - tries to process all the data available in the allocated time frame, and then runs again with the newly arrived data.
	1. 
3. Triggeronce - processes all the available data at once in one batch.
4. availableNow - processes all available data (till when this command is executed) in multiple smaller batches then stops.