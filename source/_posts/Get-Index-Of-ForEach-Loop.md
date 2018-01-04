title: Get the index of foreach loop
date: 2015-12-30 17:46:32
tags: [C#, Linq]
categories: [.NET, LINQ]
---
<a href="https://msdn.microsoft.com/en-us/library/bb534869%28v=vs.100%29.aspx">Select method (Sysmte.Linq namespace)</a> has an overload to return the loop index.

With calling this overload, the index of the loop can be get without a temporary variable.

### C# code
<pre class="brush: c-sharp;">
var array = new[] { "A", "B", "C" };

foreach (var item in array.Select((v, i) => new { v, i }))
{
	Console.WriteLine("value = {0}, index = {1}", item.v, item.i);
}
</pre>


### Result
<pre class="brush: bash;">
value = A, index = 0
value = B, index = 1
value = C, index = 2
</pre>	