title: AWS - Redshift
date: 2020-12-31 11:37:23
tags:
- AWS
- Redshift
---


# Reference
>
https://youtu.be/lj8oaSpCFTc


## Terminology 

* massively parallel, share Nothing Columnar architecture

## Best Practices: Encoding & Compression

>
https://youtu.be/lj8oaSpCFTc?t=657

* Use AZt4

### Basics 

* blocks (1MB immutable block encoded with 1 encoding)
* zone maps
* sort key

>
https://youtu.be/lj8oaSpCFTc?t=787

## Best Practices: Sort Keys 

>
https://youtu.be/lj8oaSpCFTc?t=941

* Compound key: Lowest cardinality columns first 
* Use script to help find the sort key 
* Define sort key on large table, four or less columns 

## Best Practice: Materialize columns

>
https://youtu.be/lj8oaSpCFTc?t=1001

* Frequently filtered and unchanging dimension values should be materialized within fact tables; 

## Basics: Slice, Data Distribution

>
https://youtu.be/lj8oaSpCFTc?t=1114

## Best practices: table design summary

>
https://youtu.be/lj8oaSpCFTc?t=1455

