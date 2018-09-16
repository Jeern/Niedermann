---
title: "Guidance Automation 2010 Bug"
date: 2010-08-31T02:00:00+02:00
draft: true
aliases:
    - /2010/08/31/GuidanceAutomation2010BugTheImportedProjectCMicrosoftPracticesRecipeFrameworkBuildtargetsWasNotFound.aspx
---

## Guidance Automation 2010 Bug: The imported project "C:\Microsoft.Practices.RecipeFramework.Build.targets" was not found

The new Guidance Automation Framework GAX 2010 and GAT 2010 which is installed through the Extension Manager in Visual Studio 2010, provides a nicer usability experience compared to the Visual Studio 2008 predecessor. It is installed using the new vsix installer. A vsix install is much nicer than the old msi install since it integrates into Visual Studio. You can uninstall using the Extension Manager and if a new version comes out it should appear on the Updates tab of the extension manager.

Another nice thing is that a generated factory template solution now only consists of one project which itself results in a vsix file. So the installer of your own generated factories will also be vsix files. Nice…

But nicest of all is the fact that it is automatically integrated with the Visual Studio 2010 Experimental Instance (formerly known as Experimental Hive). Before you had to manually edit your recipes to get them to register in the Experimental Hive. But know you just press Ctrl+F5 and the Experimental Instance is automatically launched with your factory installed. It is now almost easy to debug your recipes. :)

But apparently there are few weird bugs in GAX/GAT 2010. One I run into all the time is this error:

> Unable to read the project file 'Something.csproj'.
> 
> C:\Something\Something\Something.csproj(354,3): The imported project
> "C:\Microsoft.Practices.RecipeFramework.Build.targets" was not found. Confirm that the path in the <Import> declaration 
> is correct, and that the file exists on disk.

Which happens when I try to open a Guidance Automation solution.

For some reason the declaration of the variable RecipeFramework in the project file is not provided. This I deal with by editing the Project file by hand and adding a declaration to the end of the first PropertyGroup like this:

```xml
<RecipeFrameworkPath>$(DevEnvDir)Extensions\Microsoft patterns and practices\GAX 2010\2.0.20406.0</RecipeFrameworkPath>  
```

After applying this fix I can open the solution.

I have read somewhere that it is important to adhere to the following install order:

Visual Studio 2010 Ultimate or Professional (obviously)
The Visual Studio 2010 SDK
GAX 2010
GAT 2010
And this might be what I have screwed up on the particular machine where I get the error.

Anyway the above mentioned fix works. And overall I am very satisfied with the new improvements in GAX/GAT.