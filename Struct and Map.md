**STRUCT**
Struct datatype is used when we want to create  a fixed schema. Not exactly mapping but can be used as one.
- Field names are **fixed**
- Each field has a **specific type**
- Access using **dot notation** - col1.field

Col1, col2, col3 are the default names given to keys when using struct.
![[Pasted image 20260324125251.png|160]]

For defining key name,
we can use named_struct or specify key name using AS keyword:

1. **named_struct()**
![[Pasted image 20260324125803.png|173]]

2. as
![[Pasted image 20260324125846.png|155]]

## MAP
- Mapping any datatype to any datatype 
```
SELECT map(array('a','b','c'), 1, array('a','b','r'), 4) as mapping

```
Works more like a dictionary.
- Keys are **dynamic**
- All keys must be the **same type**
- All values must be the **same type**
- Access using **key lookup** - mapping[('a','b','c')] -> 1


