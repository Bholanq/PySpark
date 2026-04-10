## What is Arrow?

**Apache Arrow** is:
- A **columnar in-memory data format**
- Used for **fast data transfer between JVM (Spark) and Python**

In your pandas UDF flow:
Spark (JVM) → Arrow → Python → pandas Series
Arrow is the **bridge**, not what you code against.

## What is a Series?

**`pd.Series`** is:
- A **pandas data structure**
- Represents a **single column of actual values**
- What your function directly works on

```
col1: pd.Series
[10.0, 20.0, 30.0]
```

| Feature         | Arrow                 | Series            |
| --------------- | --------------------- | ----------------- |
| Type            | Data format           | Data structure    |
| Purpose         | Data transfer         | Data processing   |
| Used by         | Spark + Python bridge | pandas / your UDF |
| You write code? | No                    | Yes               |
| Visible to you? | Mostly hidden         | Directly used     |
# Type hinting in python 


print_func(col1: ps.Series, col2: ps.Series) -> ps.Series
### What each part means:

- `col1: ps.Series` → parameter `col1` is expected to be a `pandas-on-Spark Series`
- `col2: ps.Series` → same for `col2`
- `-> ps.Series` → the function is expected to return a `ps.Series`
- They **don’t enforce types at runtime** (Python won’t throw an error automatically)
- They are mainly used for:
    - Better readability
    - IDE autocomplete
    - Static type checkers like `mypy`

### 1. Regular Python UDF (slowest)

@udf("double")  
def f(x):  
    return x * 1.1

- Row-by-row execution
- Heavy JVM ↔ Python calls
- **Very slow**

---

### 2. Pandas UDF (better, but not best)

@pandas_udf("double")  
def f(col: pd.Series) -> pd.Series:  
    return col * 1.1

- Vectorized (batch processing)
- Uses Arrow (faster transfer)
- **Much faster than regular UDF**
- BUT still involves:
    - JVM ↔ Python transfer
    - Python execution

---

### 3. Native Spark function (fastest)

df.withColumn("new_price", col("price") * 1.1)

- Fully inside Spark engine
- Optimized by Catalyst
- No serialization
- **Fastest**