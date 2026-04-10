- Operates on **batches (columns / series)**
- Uses **Apache Arrow** for data transfer
- Processes data in **vectorized form**

### Types of PandasUDFs(Vectorized)

### 1. Series to Series 
```
import pyspark.pandas as ps  
import pandas as pd

@pandas_udf('double')
def pandas_price_func(col1: pd.Series, col2:pd.Series) -> pd.Series:
  return col1*col2
def spark_price_func(col1: pd.Series, col2:pd.Series) -> pd.Series:
  return col1*col2
  
df_ = df_.withColumn("new_price", pandas_price_func(col("price"), lit(1.1)))
display(df_)
```

### 2. iterator Series to iterator Series

This is a more advanced, performance-oriented version of pandas UDFs.

Instead of this (standard pandas UDF):

```
@pandas_udf("double")  
def f(col: pd.Series) -> pd.Series:  
    return col * 2
```

You use:

```
from typing import Iterator  
  
@pandas_udf("double")  
def f(it: Iterator[pd.Series]) -> Iterator[pd.Series]:  
    for series in it:  
        yield series * 2
```

 **Batch is the chunk, Series is the container holding that chunk**. Batch and series are used interchangeably.
 
![[Pasted image 20260405184625.png|395]]

First, spark breaks down the data into, multiple batches/series. 

Suppose the spark column is:
[10, 20, 30, 40, 50, 60]

Spark would break that down to:
it = Iterator of:
    Series([10, 20, 30])
    Series([40, 50, 60])

In case of any complex, ml models or computations, in series-to-series we'd have to load the ml_model every series iteration, but with iteration-to-iteration we only need to load the model once.

TLDR: Series to series calls the function multiple times for the individual batches/series themselves,
but Iterator to Iterator calls the function once for the iterator of the batches/series, so memory is stored and the model doesn't have to be loaded multiple times.


# Explanation:

#### 1. Series → Series UDF

```
@pandas_udf("double")  
def f(series: pd.Series) -> pd.Series:  
    model = load(...)  
    return model.predict(series.to_frame())
```

#### What Spark does internally:

Call f(batch1)  
Call f(batch2)  
Call f(batch3)  
...

Each call is **independent**.

So:

- New function call
- New Python context
- No memory of previous calls

That’s why:

model = load(...) → runs every time

---

#### 2. Iterator → Iterator UDF

```
@pandas_udf("double")  
def f(it: Iterator[pd.Series]) -> Iterator[pd.Series]:  
    model = load(...)  
    for series in it:  
        yield model.predict(series.to_frame())
```
#### What Spark does:

Call f(iterator_of_batches)

ONLY ONCE per partition.

Inside that one call:

series1 → series2 → series3 → ...

So:

- Same function execution
- Same memory
- Model reused

---

#### Why you "have to" reload in Series → Series

Because:

> **Each batch is processed in a separate function call, and state is not preserved across calls**

Even though:

- Data is still batches (Series)
- You don’t get control over multiple batches together

## 3. Multiple Iterator series to iterator series

Same a iterator to iterator, but here we use multiple iterators

```
from typing import Iterator, Tuple
import pandas as pd
from pyspark.sql.functions import pandas_udf

@pandas_udf("double")
def compute_total(it: Iterator[Tuple[pd.Series, pd.Series]]) -> Iterator[pd.Series]:
    
    # Load once
    factor = 1.1
    
    for price, tax in it:
        # price → Series (batch of price column)
        # tax   → Series (batch of tax column)
        
        total = (price + tax) * factor
        yield total
```

## Series to Scalar

```
@pandas_udf('double')
def scalar_func(col1:pd.Series)->float:
    return col1.mean()
    
    
df_ = df_.groupBy("customer_id").agg(scalar_func(col("price")).alias("avg_price"))
display(df_)
```