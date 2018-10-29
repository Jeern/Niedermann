---
title: "How to maintain more than one WMAppManifest.xml files for different versions of your Windows Phone App"
date: 2011-07-24T01:00:00+01:00
draft: false
aliases:
    - /2011/07/24/HowToMaintainMoreThanOneWMAppManifestxmlFilesForDifferentVersionsOfYourWindowsPhoneApp.aspx
---
When we were to publish a free version of our new Windows Phone Game [Photo Challenge](http://windowsphone.com/s?appid=ab75a012-53a3-e011-986b-78e7d1fa76f8) I wanted to maintain both the free and paid version in the same Repository. I wanted the solution file and project file for the new free app to use the same source files as the paid app. This gave some headaches because there needs to be a physical WMAppManifest.xml file for each App. I googled various approaches but I finally made up an entirely different approach which I thought fitted our project best. Here is the steps I took:

* Copied the solution file PhotoChallenge.sln to PhotoChallengeFree.sln – placed in the same folder
* Copied the project file PhotoChallenge.csproj to PhotoChallengeFree.csproj – placed in the same folder
* Changed PhotoChallengeFree.sln via notepad to refer to PhotoChallengeFree.csproj
* Copied AssemblyInfo.cs to two new folders. One for the paid app and one for the free app.
* Deleted AssemblyInfo.cs from both PhotoChallenge.csproj and PhotoChallengeFree.csproj
* From PhotoChallenge.csproj I linked to the new Paid version of AssemblyInfo.cs – using Add –> Existing File, but choosing Add As Link instead of Add.
* The same in PhotoChallengeFree.csproj. But this time linking to the Free version of AssemblyInfo.cs
* Changed the following properties in the free version of AssemblyInfo.cs: AssemblyTitle, AssemblyProduct and Guid.

I actually wanted to manipulate WMAppManifest.xml in the same way as I did AssemblyInfo.cs. But this turned out to be impossible. The WMAppManifest.xml file has to be physical – not a link. Instead I did this:

* Made a project folder in PhotoChallenge.csproj called PropertiesPaid
* Made a project folder in PhotoChallengeFree.csproj called PropertiesFree
* Copied WMAppManifest.xml to the new folders and added the relevant one to each project.
* Deleted WMAppManifest.xml from the properties folder.
* Opened the project files in Notepad (actually I chose “Unload Project” in Visual Studio and then “Edit PhotoChallenge.csproj”). And changed this:

```xml

<XnaWindowsPhoneManifestTemplate>Properties\WMAppManifest.xml</XnaWindowsPhoneManifestTemplate>  
```

To this:

```xml

<XnaWindowsPhoneManifestTemplate>PropertiesPaid\WMAppManifest.xml</XnaWindowsPhoneManifestTemplate> 
```

Then I reloaded the project in Visual Studio, and did the same for the free project. But now refering to the PropertiesFree folder.

In the free version of WMAppManifest.xml I changed the Title and TokenId to match the AssemblyTitle and AssemblyProduct of AssemblyInfo.cs. Furthermore I changed the ProductId to match the Guid of the AssemblyInfo.cs.

The actual differences between the free and paid version in the source file I maintain using a new compilation symbol I made called “FREE” so I can make statements like this:

```csharp
#if FREE  
//Some code  
#endif  
```

Altogether my approach seems to work extremely well to minimize maintainance of the two versions of the same app.

One small problem: For some reason which I have not bothered to find out I cannot use the Compilation symbol Free in the PhotoChallenge.csproj project itself, but I have some other referenced projects where it is no problem. So I just have a class in the referenced project which I can use where ever I have to check if the current version is paid or free:

```csharp
public static class Edition  
{      
    public static bool IsFree  
    {          
        get  
        {   
#if FREE              
            return true;   
#else              
            return false;  
#endif          
         }      
     }  
}  
```