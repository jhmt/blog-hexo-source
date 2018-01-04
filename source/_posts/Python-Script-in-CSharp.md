title: Python Script in C#
date: 2015-12-07 16:01:39
tags: [Python, IronPython, C#]
categories: [Python, IronPython]
---
Python Script can be called from C# with using <a href="https://www.nuget.org/packages/IronPython">IronPython</a>. 

### Python Script
<pre class="brush: python;">
def hello(name) :
    return "Hello! " + name
	
</pre>

### C# code
<pre class="brush: c-sharp;">
using IronPython.Hosting;
using Microsoft.Scripting.Hosting;

void HelloPython
{
    ScriptEngine python = Python.CreateEngine();
    string path = @"C:\scripts\hello.py";
	
    dynamic script = python.ExecuteFile(path);
	Console.WriteLine(script.hello("John"));
}
</pre>

<pre class="brush: bash;">
> Hello! John
</pre>

This can be used in **cshtml** view files like this:

<pre class="brush: c-sharp;">
@using IronPython.Hosting;
@using Microsoft.Scripting.Hosting;

@{
    ScriptEngine python = Python.CreateEngine();
    string path @"C:\scripts\hello.py";
    
    dynamic script = python.ExecuteFile(path);
    string msg = script.hello("John");
}

&lt;h2>@msg&lt;/h2>
</pre>