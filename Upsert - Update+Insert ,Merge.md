![[Pasted image 20260323145327.png|431]]

```
from delta.tables import *
dlt_obj = DeltaTable.forPath(spark, "/Volumes/my_catalog/raw/source/destination/delta/")
dlt_obj.alias("tgt").merge(df_new.alias("srs"),"t.order_id = s.order_id")\
    .whenMatchedUpdateAll()\
    .whenNotMatchedInsertAll()\
    .execute()
```

Upsert using the merge() command, updates the row if it already exists in the col, otherwise it insert the new row. 
The columns are identified using some condition