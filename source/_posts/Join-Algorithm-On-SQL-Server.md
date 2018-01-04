title: Join Algorithm on SQL Server
date: 2016-01-02 19:57:54
tags: [Database, SQL, SQL Server, Performance Tuning]
categories: [Database, SQL Server]
---
<a href="https://msdn.microsoft.com/en-us/library/ms173815.aspx">Join Hints</a> enforce query optimizer to use a specified join algorithm.

The characteristics of the algorithms are as follows:

| Name         | Pros                                                              | Cons |
|--------------|-------------------------------------------------------------------|------|
| *Nested Loops* | - Very fast on "Small driven table"  <br/>- Less consume of memory and disk (suitable for OLTP) <br/>- Available for non-equijoins joins | - Unsuitable for large tables join   |
| *Hash*         | - Suitable for large tables join | - Expensive for memory (unsuitable for OLTP)<br/>- Unavailable for non-equijoins joins    |
| *Sort Merge*   | - Suitable for large tables join<br/>- Available for non-equijoins joins  |  - Expensive for memory (unsuitable for OLTP)<br/>- Unavailable for non-equijoins joins<br/>- Inefficient fro unsorted tables.
	
### Optimum algorithms
Based on table size, the proper algorithms for each join query are as follows: 

1. Small to Small
"Nested Loops"

2. Small to Large
"Nested Loops" with using an index of the large table's join key.
In case of many rows, exchange the driven table from the small one to the large one. Or "Hash" algorithm might be better solution.

3. Large to Large
"Hash" should be the first try. If the join key has been sorted, then try "Sort Merge"