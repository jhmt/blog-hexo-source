title: Performance Tuning with Window Function
date: 2016-01-04 19:36:41
tags: [Database, SQL, SQL Performance Tuning]
categories: [Database, Performance Tuning]
---

Thinking of the best solution to get the product of each group which were registered earliest.

### Product Table

<pre class="brush: plain;">
| ProductId  | GroupId | RegisterDate | Price   |
|------------|---------|--------------|---------|
|      1     |   101   |  2016-01-01  | 250.00  |
|      2     |   101   |  2016-02-01  | 1200.00 |
|      3     |   101   |  2016-01-23  | 70.00   |
|      4     |   201   |  2016-01-14  | 800.00  |
|      5     |   201   |  2016-01-31  | 20.00   |
|      6     |   301   |  2016-10-01  | 25.00   |
|      7     |   301   |  2016-05-04  | 900.00  |
|      8     |   301   |  2016-12-01  | 750.00  |
|      9     |   301   |  2016-01-31  | 890.00  |
|     10     |   301   |  2016-11-11  | 750.00  |
|     11     |   401   |  2016-08-25  | 100.00  |
</pre>

### Expected Result

<pre class="brush: plain;">
| GroupId | RegisterDate | Price  |
|---------|--------------|--------|
|   101   |  2016-01-01  | 250.00 |
|   201   |  2016-01-14  | 800.00 |
|   301   |  2016-01-31  | 890.00 |
|   401   |  2016-08-25  | 100.00 |
</pre>

### Solution 1

The easiest way to solve this is to use Join clause like this:

<pre class="brush: sql;">

SELECT p1.GroupId, p1.RegisterDate, p1.Price
FROM Product AS p1
INNER JOIN (SELECT GroupId, MIN(RegisterDate) AS [MinDate] FROM Product GROUP BY GroupId) AS p2
ON p1.GroupId = p2.GroupId AND p1.RegisterDate = p2.MinDate;

</pre>

*Solution 1* has a performance problem because of following 4 reasons:

* Sub queries tend to be stored in a temporary area (memory, disks, etc...). This will cause an overhead.
* Sub queries don't have indexes or constraint to optimize.
* Join queries are more often than not expensive and have risks - Execute plan might be changed according to the scalability.
* 2 scans to the table.


### Avoid Join Query with Window Function

In this case, **"ROW_NUMBER"** window function enables us to avoid using join query. 

#### Solution 2

<pre class="brush: sql;">

SELECT p.GroupId, p.RegisterDate, p.Price
FROM 
(SELECT GroupId, RegisterDate, Price, 
 ROW_NUMBER() OVER (PARTITION BY GroupId ORDER BY RegisterDate) AS [rowNum]
FROM Product) AS p
WHERE p.rowNum = 1

</pre>





