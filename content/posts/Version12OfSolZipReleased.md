---
title: "Version 1.2 of SolZip released"
date: 2009-07-15T01:00:00+01:00
draft: false
aliases:
    - /2009/07/15/Version12OfSolZipReleased.aspx
---
Just released version 1.2 of [SolZip](http://solzip.codeplex.com/) my convenient tool for Zipping Visual Studio 2008 solutions

The major improvement is a single install for SolZipMME, so you won't have to install [ManagedMenuExtensions](http://managedmenuextension.codeplex.com/) beforehand.

The release notes are here:

1. Bug Fix: when multiple projects where referering to the same file it was added to the archive multiple times.
1. SolZipMME uses SaveFileDialog instead of FolderBrowserDialog for Zip files.
1. A single file installer for SolZipMME.
1. Clipboard functionality now works on Vista / Windows Server 2008.
1. SolZipGuidance changed. The menu will no longer appear on projects that are not C# projects. SolZipMME not changed.
1. Support for $(SolutionDir) and $(ProjectDir) placeholders added
1. Now works on Windows Vista even if UAC is not turned off

Enjoy...