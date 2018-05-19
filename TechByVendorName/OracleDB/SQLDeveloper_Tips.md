title: Data Migration using SQL Developer Tool (Oracle)
date: 2018-05-14 07:53:33
tags:
- SQL Developer
- Data Migration
---

# Senario

Move >10k records from MS SQL Database A to B (Different Table Design)

```sql
spool "C:\temp\data.csv"

select /*csv*/
    name as userName,
    address as userAddress
from customers;

spool off;
```

The records will be written into csv for furthur processing

* Suitable for temp solution or inital load
* Data exported with "" , __which I haven't found a way to get rid of__; I am using Java to strip off the "" before the next processing steps
