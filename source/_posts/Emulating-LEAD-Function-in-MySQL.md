title: Emulating LEAD Function in MySQL
date: 2015-11-06 09:50:59
tags: [Database, SQL, MySQL]
categories: [Database, MySQL]
---
<a href="https://msdn.microsoft.com/en-us/library/hh213125.aspx">LEAD</a> function can be emulated in MySQL with using **LIMIT** clause.

## Example:
<pre class="brush: plain;">
|  Id  |  Batch_Id  | Start Date | End Date
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

### Update with Emulated LEAD Function in MySQL
<pre class="brush: sql;">
UPDATE Batch_Log bl
INNER JOIN 
(select 
 Id, 
 (select b.Date_Start
  from Batch_Log b
  where b.Batch_Id=a.Batch_Id
    and b.Date_Start > a.Date_Start
  order by b.Date_Start Limit 1) as endDate
  from Batch_Log a
  order by Id, Date_Start) lead
ON bl.Id = lead.Id
SET bl.Date_End = lead.endDate
;
</pre>

<a href="http://sqlfiddle.com/#!9/87fc3/1">SQL Fiddle</a>