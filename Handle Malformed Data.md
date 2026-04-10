CORRUPT RECORD MODES 

1. Permissive
		.option("mode","permissive")
		-If there are some malformed/corrupt data it will store the data in another record

2. Drop Malformed
		.option("mode","DROPMALFORMED")

3. FAILFAST
		.options("mode","FAILFAST")