---
description: This page archived some classical questions related to database.
---

# Database

## What is index and its function? 

Indexing is a way of **sorting a number of records on multiple fields**. Creating an index on a field in a table creates another data structure which holds the field value, and a pointer to the record it relates to. This index structure is then sorted, **allowing Binary Searches to be performed on it.**

Index is used to optimize the storage of table records and help quick retrieval of records. By using sql index, it's easily to sort and search table.   
A primary key is also always an index key.

In database, we used B+tree and B tree to implement the index. B tree stored the records as internal as well as leaf notes, whereas in B+ tree, the records are sorted as leaf nodes, and the keys are stored only in internal nodes. 

Advantage of B+ tree: 

1\) Faster to do the queries, and do the full scan. 

2\) Keys are used for indexing. 

Explain: In Postgre Explain, it showed the query plan. 

## 

Yes, after a few months we finally found the answer. Sadly, Mike is on vacations right now so I'm afraid we are not able to provide the answer at this point.



