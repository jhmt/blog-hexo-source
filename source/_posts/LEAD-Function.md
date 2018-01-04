title: LEAD Function
date: 2015-11-05 18:35:08
tags: [Database, SQL, SQL Server]
categories: [Database, SQL Server]
---
<a href="https://msdn.microsoft.com/en-us/library/hh213125.aspx">LEAD</a> function is one of the best options to update **PREVIOUS** records. (SQL Server 2012 or newer version required)

## Example:
<pre class="brush: plain;">
|  Id  |  BatchId  | Start Date | End Date
-------------------------------------------
|  1   |  bat1     | 2015-11-01 |
|  2   |  bat1     | 2015-11-04 |
|  3   |  bat2     | 2015-11-02 |
|  4   |  bat2     | 2015-11-05 |
|  5   |  bat3     | 2015-11-03 |
|  6   |  bat3     | 2015-11-06 |
|  7   |  bat2     | 2015-11-10 |
</pre>

**End Date** column of each record needs to be updated with **Start Date** values of **NEXT** records.

### Update with LEAD Function
<pre class="brush: sql;">
UPDATE b
SET b.EndDate = lead.StartDate
FROM BatchLog AS b
INNER JOIN 
(
  SELECT 
  Id
  , LEAD(StartDate) OVER (PARTITION BY BatchId ORDER BY StartDate) LeadDate
  FROM BatchLog
) AS lead ON b.Id = lead.Id
</pre>

<a href="http://sqlfiddle.com/#!6/f7e08/3">SQL Fiddle</a>