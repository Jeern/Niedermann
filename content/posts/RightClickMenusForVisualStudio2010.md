---
title: "Right Click menus for Visual Studio 2010"
date: 2010-06-13T01:00:00+01:00
draft: true
aliases:
    - /2010/06/13/RightClickMenusForVisualStudio2010.aspx
---
Imagine that you want to make an extension for Visual Studio 2010 that creates new custom right click menus for the Solution Explorer. Imagine that you could do it by just implementing a good old .NET interface like this:

```csharp
public class MenuManager : IMenuManager  
{  
    public IEnumerable<IMenuItem> GetMenus(ContextLevels menuForLevel)  
    {  
        var menuItems = new List<IMenuItem>();  
        var menuItem1 = new MenuItem("My Menu1");  
        menuItems.Add(menuItem1);  
        var menuItem2 = new MenuItem("My Menu2");  
        menuItems.Add(menuItem2);  
        return menuItems;  
    }  
  
    public string MainMenu()  
    {  
        return "My Main Menu";  
    }  
}  
```

Well it turns out you can. In [MME](http://mme.codeplex.com/) I have helped you do exactly that. No more using the convoluted add-in model of Visual Studio to accomplish this goal. And it is also much simpler than using GAX/GAT (that can do so much more to be fair).

You can easily install MME, either by downloading directly from [Codeplex](http://mme.codeplex.com/) or by installing directly from the Extension Manager in Visual Studio 2010 (you can find it under the Tools menu).

MME does not work for the Express editions of VS.

I also recommend installing the MME MenuManager template which you can also find in the Extension Manager.

At codeplex you can read more about [implementing](http://mme.codeplex.com/wikipage?title=implementing&referringTitle=Documentation) and [deploying](http://mme.codeplex.com/wikipage?title=deploying&amp;amp;referringTitle=Documentation) MME’s and also get further insight on the [architecture](http://mme.codeplex.com/wikipage?title=Architecture&amp;amp;referringTitle=Documentation).