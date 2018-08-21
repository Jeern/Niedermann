---
title: "Execute method for Linq"
date: 2009-05-01T01:00:00+01:00
draft: true
aliases:
    - /2009/05/01/ExecuteMethodForLinq.aspx
---
If you want to Execute something on each element of the result of a Linq expression. You can either loop over the result using foreach(...) or turn the result into a List using .ToList(), and then use .Foreach(...) on the resulting list. Like this:

```csharp
(from excep in ErrorList  
where excep is ArgumentException select excep)  
    .ToList().ForEach(e => Console.WriteLine(e.Message));
```

This is ugly, and I prefer to make my own Extension method for `IEnumerable<T>` called `Execute(...)`:

```csharp
public static void Execute<T>(this IEnumerable<T> list, Action<T> action)  
{  
    foreach(T element in list)  
    {  
        action(element);  
    }  
}  
```

This means you can do this instead:

```csharp
(from excep in ErrorList  
where excep is ArgumentException select excep)  
.Execute(e => Console.WriteLine(e.Message));
```

Back in the days when I used Linq in a program for the first time, I wondered why there was no way to execute an action on each element of a list, so I implemented the Execute method.

It was only later that I discovered that the default way was .ToList().Foreach(...)

I must say I still prefer my original approach.

BTW: In a small none scientific test I have tested the two approaches.

With Linq To Objects iterating over 100000 integers and printing them to the Console takes about 21 seconds with the Foreach method and about 20 seconds with the Execute method, so it is even faster at least for this particular example. Though nothing to get excited about. :O)

The test was done in a Vista VPC with VS2008,

Small performance test: [LinqPerfTest.zip](/resources/ExecuteMethodForLinq/LinqPerfTest.zip)