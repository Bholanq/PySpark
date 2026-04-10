Is a command line tool used to submit a spark application to a cluster

eg.  submitting a word count code to spark. (wordcount.py)

spark-submit \
	--master yarn \
	--deploy-mode cluster \
	--num-executors 4 \
	--executor-memory 4G \
	--executor-cores 2 \
	 wordcount.py