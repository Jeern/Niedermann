---
title: "Version 1.1 of SolZip released"
date: 2009-06-29T01:00:00+01:00
draft: true
aliases:
    - /2009/06/29/Version11OfSolZipReleased.aspx
---
I just released version 1.1 of [SolZip](http://solzip.codeplex.com/) which can be downloaded from the [download page](http://solzip.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=29427) at the SolZip site.

The new action packed version contains one major improvement, one bug fix, and a few name changes:

Now Source Control bindings for TFS and SCC can be removed from sln and csproj files. The default for all 3 UI's is that the bindings are removed, but you can optionally choose not to remove them. Feel free to contact me if you need bindings from other source control vendors removed.
A bug fixed that made WinZip clients give a warning when unzipping certain archives made by SolZip. The warning was given when there was .. (2 dots) in a path in one of the zipped files.
The Name SunZip.exe for the commandline tool was changed to SolZip.exe. Because SunZip sounded too much like UnZip.
The Name SolutionZipper changed to SolZip everywhere. Except for SolutionZipper.dll which has been changed to SolZipBasis.dll
In the next version I will try to include [ManagedMenuExtensions](http://managedmenuextension.codeplex.com/) in the install so you won't have to run more than one setup to install SolZipMME.

Enjoy the new version :O)