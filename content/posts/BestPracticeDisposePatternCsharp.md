---
title: "Best practice Dispose Pattern C#
"
date: 2009-06-18T01:00:00+01:00
draft: false
aliases:
    - /2009/06/18/BestPracticeDisposePatternC.aspx
---
Here is my take on the Best practice Dispose pattern for most situations. It is heavily influenced by Juval Löwy from IDesigns book ["Programming .NET Components"](http://www.amazon.co.uk/Programming-NET-Components-Applications-Component-Oriented/dp/0596102070/ref=sr_1_2?ie=UTF8&amp;amp;s=books&amp;amp;qid=1245356943&amp;amp;sr=8-2), and some other sources which I no longer remember, since I made it a few years ago and have stuck to it ever since. Here it is:

```csharp
public class Class1 : IDisposable
{
    private bool m_IsDisposed;

    ~Class1()
    {
        Dispose(false);
    }


    [MethodImpl(MethodImplOptions.Synchronized)]
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    private void Dispose(bool isDisposing)
    {
        if (m_IsDisposed)
            return;

        if (isDisposing)
        {
            FreeManagedRessources();
        }
        FreeUnmanagedRessources();
        m_IsDisposed = true;
    }

    protected virtual void FreeManagedRessources()
    {
        //Free managed ressources here. Typically by calling Dispose on them
    }

    protected virtual void FreeUnmanagedRessources()
    {
        //Free unmanaged ressources here.
    }
} 
```

Another thing is that you can inherit the class and override the FreeManagedRessources and FreeUnmanagedRessources as needed.