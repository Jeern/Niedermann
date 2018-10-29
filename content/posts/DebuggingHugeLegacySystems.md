---
title: "Debugging huge legacy systems"
date: 2009-03-11T01:00:00+01:00
draft: false
aliases:
    - /2009/03/11/DebuggingHugeLegacySystems.aspx
---
Currently I am looking into taking over a huge enterprise legacy system written mostly in C#, but also some C++. The numbers are staggering several million lines of code, 1200 GUI screen, mostly WinForms, about 750 C# projects, 150 solution files, just to mention a few. In all about 4 times larger than the largest enterprise system I have previously been working on.

Just building and deploying the system is a pretty big task since due to events beyond my control I basicly only have the sourcecode and nothing else. Not quite true because there are some documentation, but buried in the thousands of files.

When I managed to modify a build file and build the entire monster with it, it took 2½ hours to complete.

I won't even try to include all projects in a single Visual Studio file. I wonder if it would load ? This gives me a problem though. How about debugging ? I cannot press F5 for any of the visual studio projects because the build process using the build file set up a ton of config files with IP adresses, connection strings and the like. So it doesn't make sense to run the thing from Visual Studio.

But if I start up the exe assembly how do I attach a debugger ? Well, I decided to use a simple trick. I added a line of code in the top of the main() method:

```csharp
static void Main(string[] args)   
{  
  Debugger.Break();  
  ...  
```

Debugger.Break() is basicly a programmatic breakpoint, but it is more than that. If no debugger is attached it asks the user for a debugger. Smart! And for the process to debug I can just choose the Visual Studio process with the project containing the exe assembly. Henceforth the control is mine.

I could even consider writing this method and using it instead:

```csharp
[Conditional("DEBUG")]  
private static void DebuggerBreak()  
{  
    if(!Debugger.IsAttached)  
        Debugger.Break();  
}  
```

This gives me the advantage that the IL code is only build in DEBUG mode and that the Debugger is only attached if one is not attached already.