title: Database access from Razor view through IronPython
date: 2015-12-12 22:58:34
tags: [Python, IronPython, C#, Razor]
categories: [Python, IronPython]
---
With IronPython, razor engine views can access to Database directly.

### Razor View
<pre class="brush: c-sharp;">
@using IronPython.Hosting;
@using Microsoft.Scripting.Hosting;

@{
    ScriptEngine python = Python.CreateEngine();
    string path @"C:\scripts\hello.py";
    
    dynamic script = python.ExecuteFile(path);
    string msg = script.hello(1);
}

&lt;h2>@msg&lt;/h2>
</pre>


### Python Script
<pre class="brush: python;">
import clr
clr.AddReference('System.Data')
from System.Data import *

def hello(id) :
    conn = SqlClient.SqlConnection("server=SERVER_ADDRESS;database=MyDB;uid=sa;password=PASSWORD")
    conn.Open()

    cmd = SqlClient.SqlCommand("SELECT Name FROM Member WHERE Id = @Id", conn)
    cmd.Parameters.Add("@Id", SqlDbType.Int).Value = id;
    reader = cmd.ExecuteReader()

    name = ''
    reader.Read()
    name = reader[0]

    reader.Close()
    conn.Close()

    return "Hello!" + name
</pre>
