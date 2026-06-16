`ZORDER` is a **data skipping optimization**.
It reorganizes data so that rows with similar values for specified columns are stored close together.

---

Suppose your table looks like this:

|customer_id|country|
|---|---|
|101|US|
|500|India|
|12|Germany|
|101|US|
|500|India|

Without ZORDER, these rows may be scattered across files.

---

If most queries are:

```
SELECT *FROM salesWHERE customer_id = 101;
```

Databricks might have to read many files.

---

## Syntax

```
OPTIMIZE salesZORDER BY (customer_id);
```

---

## What happens?

Data gets reorganized so that similar customer IDs are colocated.

Example:

```
File 1 → customer_id 1–1000File 2 → customer_id 1001–2000File 3 → customer_id 2001–3000
```

Now Delta's statistics can skip irrelevant files.