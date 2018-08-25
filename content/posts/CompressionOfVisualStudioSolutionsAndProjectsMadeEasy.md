---
title: "Compression of Visual Studio Solutions and Projects made easy"
date: 2009-06-15T01:00:00+01:00
draft: true
aliases:
    - /2009/06/15/CompressionOfVisualStudioSolutionsAndProjectsMadeEasy.aspx
---
I just released a new codeplex project [SolutionZipper](http://solzip.codeplex.com/) which makes it a breeze to zip compress your visual studio solutions and projects. "What's wrong with WinZip you might Ask". Well WinZip has no knowledge of Visual Studio Solutions, which means it will compress everything in the folder, including bin / obj folders and other random debris.

I accomplish the compression by using the following algorithm:

1. Iterate over all items and projects in the sln file and zip each of these.
1. Zip the sln file itself.
1. Iterate over all items in the csproj files and zip each of these. The iteration is done using Linq to Xml of course.
1. Zip the csproj file itself.

As you might have guessed SolutionZipper only works for C# projects, and is only tested with VS2008.

I offer three UI's for SolutionZipper

[SunZip](http://solzip.codeplex.com/Wiki/View.aspx?title=SunZip) - A commandline tool.

 

[SolZipMME](http://solzip.codeplex.com/Wiki/View.aspx?title=SolZipMME) - Providing Right click menus for Visual Studio 2008 using [Managed Menu Extensions](http://managedmenuextension.codeplex.com).

 

[SolZipGuidance](http://solzip.codeplex.com/Wiki/View.aspx?title=SolZipGuidance) - Providing Right click menus for Visual Studio 2008 using Guidance Automation (GAX).

 

For the actual zipping I use the excellent open source framework [SharpZipLib](http://www.icsharpcode.net/OpenSource/SharpZipLib).