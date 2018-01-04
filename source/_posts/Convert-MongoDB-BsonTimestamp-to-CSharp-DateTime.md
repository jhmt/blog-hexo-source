title: "Convert MongoDB BsonTimestamp to C# DateTime"
date: 2015-11-08 11:11:30
tags: [MongoDB, C#]
categories: [Database, MongoDB]
---
MongoDB's **Timestamp** is the elapsed seconds since the Unix epoch (1970/1/1). Therefore, the conversion from Timestamp to DateTime with using <a href="https://docs.mongodb.org/ecosystem/drivers/csharp/">C# and .NET MongoDB Driver 2.0</a> is like this:

<pre class="brush: c-sharp;">
DateTime datetime = new DateTime(1970, 1, 1).AddSeconds(bsonTimestamp.Timestamp);
</pre>

### Timestamp Format
According to the <a href="https://docs.mongodb.org/manual/reference/bson-types/#document-bson-type-timestamp">MongoDB Manual 3.0</a>, the first integer value is the elapsed seconds since the Unix epoch. The second one is an ordinal number to make the Timestamp unique.

For example:
<pre class="brush: javascript;">
Timestamp(1406171938, 1) 
</pre>

So, **1406171938** is the elapsed seconds, **1** is just an ordinal number.