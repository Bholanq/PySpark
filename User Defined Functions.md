@udf(returnType = floattype)
- same  as python functions 
	`@udf(returnType=FloatType())
		def my_square_new(x):
	    square = x*x
	    return square

df_cust = df_csv.withColumn("price_sqaure",my_square_new('price'))
### User Defined Table Functions

- whenever we want to generate a data frame from some data we use udtf s.
- eg. generating a Dataframe from a string 


@udtf(returnType="word STRING")
class udtf_class:
    def eval(self, text: str): # Function name to be used
        for word in text.split():
            yield (word,)

udtf_class(lit("Hello world, this is a developer")).show()

### Call UDFs

- first we register the function, using spark.udf.register(name, function) then call the function using call_udf() 

spark.udf.register("my_square_new", my_square_new)
df_cust_new = df_csv.withColumn("price_sqaure_new",call_udf("my_square_new",col('price')))
display(df_cust_new)

