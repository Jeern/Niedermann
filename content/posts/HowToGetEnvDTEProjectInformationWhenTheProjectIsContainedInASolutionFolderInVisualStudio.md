---
title: "How to get EnvDTE.Project information when the project is contained in a Solution Folder in Visual Studio
"
date: 2009-06-14T01:00:00+01:00
draft: false
aliases:
    - /2009/06/14/HowToGetEnvDTEProjectInformationWhenTheProjectIsContainedInASolutionFolderInVisualStudio.aspx
---
I was using my codeplex project [ManagedMenuExtensions](http://managedmenuextension.codeplex.com/) recently when I realized that it threw an Exception, when clicking on a menu attached to a C# project under a solution folder. The reason was that the object which normally contained an EnvDTE.Project when a project was clicked contained null, when this project was contained in a solution folder.

I normally use this code to get at the selected project (m_VSStudio is the DTE object of the current solution):

```csharp
private UIHierarchyItem SelectedItem  
{  
  get  
  {  
    UIHierarchy uiHierarchy = m_VSStudio.ToolWindows.SolutionExplorer;  
    if(uiHierarchy == null)  
      return null;  
  
    object[] items = uiHierarchy.SelectedItems as object[];  
    if(items == null || items.Length == 0)  
      return null;  
  
    return items[0] as UIHierarchyItem;  
  }  
}
```

The SelectedItem.Object of type object now contains a EnvDTE.Solution object if the solution is selected in the solution explorer, and guess what :O) it contains an EnvDTE.Project if a project is selected. It is this SelectedItem.Object that contains null, if the project is contained in a solution folder.

I googled this problem and found others who had the same problem, but none with a solution, so I had to come up with my own. I don't know if this behaviour is by design from Microsoft, or if it is a bug.

# My Solution

Normally I would test if a project is selected with this code:

```csharp
var project = SelectedItem.Object as Project;  
if (project != null)  
{  
    //Do something with the project  
}  
```

Now I use this code instead:

```csharp
Project project = GetProject(SelectedItem.Object);  
if (project != null)  
{  
    //Do something with the project  
}  
```

Where I implemented GetProject(...) as:

```csharp
private Project GetProject(object selectedItemObject)  
{  
    var project = selectedItemObject as Project;  
    if (project != null)  
        return project;   
  
    var item = selectedItemObject as ProjectItem;  
    if (item == null)  
        return null;   
  
    return item.SubProject;  
}  
```

Not exactly beautiful, but that is to be expected when playing with Visual Studios AddIn model, and the kind of thing I try to hide in [ManagedMenuExtensions](http://managedmenuextension.codeplex.com/). I haven't had a chance to look at Visual Studio 2010 yet, one could hope that they have done a better job of hiding the ugly underlying COM stuff.

Anyway, I hope this helps the next person who tries to search for a workaround for this rather weird behaviour.
