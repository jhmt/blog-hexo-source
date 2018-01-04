title: Generate TypeScript Definition Automatically
date: 2015-12-01 21:19:25
tags: [JavaScript, TypeScript, C#]
categories: [JavaScript, TypeScript]
---
<a href="http://type.litesolutions.net/">TypeLITE</a> generates Interface definition from C# classes specified in T4 pattern.

### How To Use
Set <a href="http://type.litesolutions.net/doc/TypeLite/TsClassAttribute">TsClassAttribute</a> (+ module name) to DTO classes.

<pre class="brush: c-sharp;">
using TypeLite;

[TsClass(Module = "module1")]
public class Sample1
{
	public int Value { get; set; }
	public Enum1 Type { get; set; }
}

[TsEnum(Module = "module1")]
public enum Enum1
{
	First = 1,
	Second = 2
}
</pre>
	
### Generated Code

TypeLite.Net.d.ts
<pre class="brush: js;">
declare module module1 {
	interface Sample1 {
		Value: number;
		Type: module1.Enum1;
	}
}
</pre>

Enum.ts
<pre class="brush: js;">
module module1 {
	export enum Enum1 {
		First = 1,
		Second = 2
	}
}
</pre>