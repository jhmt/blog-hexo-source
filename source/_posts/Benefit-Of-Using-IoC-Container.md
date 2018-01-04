title: Benefit of using IoC Container
date: 2016-02-07 22:02:10
tags: [Dependency Injection, C#, Ninject]
categories: [Design Pattern, Dependency Injection]
---
Ioc Container provides some benefits when using Dependency Injection design pattern.

Let's see sample code:

### Interface and Class
<pre class="brush: c-sharp;">
public interface ISender
{
    void Send(string message);
}

public class EmailSender : ISender
{
    public void Send(string message)
    {
        Console.WriteLine("{0} has been sent via email.", message);
    }
}

public class CourierSender : ISender
{
    public void Send(string message)
    {
        Console.WriteLine("{0} has been sent via a courier.", message);
    }
}
</pre>

### Case 1: DI without Ioc Container
<pre class="brush: c-sharp;">
public class Messenger
{
    private ISender _sender;

    public Messenger(ISender sender)
    {
        _sender = sender;
    }

    public void Send(string message)
    {
        _sender.Send(message);
    }
}

class Program
{
    static void Main(string[] args)
    {
    	// Send via email
        var messenger = new Messenger(new EmailSender());
        messenger.Send("Congrats!");
        // Send via courier
        messenger = new Messenger(new CourierSender());
        messenger.Send("Congrats!");
    }
}
</pre>

This has the following problems:
- The more Messenger class is used, the more *new* intantiation. This will cause less maintenancebility.
- EmailSender and CourierSender depend on other classes. This makes instantiating them to each layer when they have hierarchy.
- Less maintenancebility to changes to EmailSender and CourierSender structures.

IoC Container provides solutions for these problems.
Following is a sample usage of IoC container, <a href="http://www.ninject.org/">*Ninject*</a>. *Ninject* is avaliable on Nuget package manager.

### Case 2: DI with Ioc Container
<pre class="brush: c-sharp;">
using Ninject;
class Program
{
    static void Main(string[] args)
    {
    	// IoC Container
        var kernel = new StandardKernel();
        // Send via email
        kernel.Bind&lt;ISender>().To&lt;EmailSender>();
        kernel.Get&lt;ISender>().Send("Congrats!");
        // Send via courier
        kernel.Rebind&lt;ISender>().To&lt;CourierSender>();
        kernel.Get&lt;ISender>().Send("Congrats!");
    }
}
</pre>

As shown in the sample code, IoC Conatiner provides some benefits:
- Centralizing instantiation
- Encapsulating class members
- More testability with using mock code 

These provide maintenancebility, flexibility and testability.
