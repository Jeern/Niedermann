---
title: "Syntax highlighting in this blog"
date: 2009-03-08T01:00:00+01:00
draft: false
aliases:
    - /2009/03/08/SyntaxHighlightingInThisBlog.aspx
---

When I post code blocks in this blog. It would be nice if they look like, well, code. So I looked at what [Scott Hanselman](http://www.hanselman.com) had done, he is after all one of the creators of the blogengine I use: ["Dasblog"](http://dasblog.codeplex.com/)

In this [post](http://www.hanselman.com/blog/BestCodeSyntaxHighlighterForSnippetsInYourBlog.aspx) Scott explains how to do it. He explains how to add Javascript from a project called [SyntaxHighlighter](http://code.google.com/p/syntaxhighlighter/) and use a plugin called [precode](http://precode.codeplex.com/) for Windows Live Writer. Well I use WLW too, so this is perfect for me.

But I had a few problems. I took me some time to figure out that the newest edition of SyntaxHighlighter didn't work with precode.

So I have now settled on SyntaxHighlighter version 1.5.1 and precode version 3.0.0

SyntaxHighlighter is nice because it has support for multiple languages, and it is easy to add support for more. The caveat is that it hasn't got any knowledge of the language. Which means that it just parses the text and looks for keywords. So stuff that looks like keywords will be color highlighted too. I consider this a minor problem.

I chose to add the Javascript in the scripts folder of my blog, and the SyntaxHighlighter.css file in template folder.

Know I could easily add the required code in the bottom of my homeTemplate.blogTemplate file, immediately before the closing <body> tag, like so:


```html
<script class="javascript" src="scripts/shCore.js"></script>  
<script class="javascript" src="scripts/shBrushCpp.js"></script>  
<script class="javascript" src="scripts/shBrushCSharp.js"></script>  
<script class="javascript" src="scripts/shBrushJScript.js"></script>  
<script class="javascript" src="scripts/shBrushSql.js"></script>  
<script class="javascript" src="scripts/shBrushXml.js"></script>  
<script class="javascript">  
    dp.SyntaxHighlighter.ClipboardSwf = 'scripts/clipboard.swf';  
    dp.SyntaxHighlighter.BloggerMode();  
    dp.SyntaxHighlighter.HighlightAll('code');   
</script>  
```

Of course if you don't use DasBlog you just put the code in the bottom of each page.

Furthermore you need to refer to the stylesheet in the top of each page.

The last thing I did was to modify the stylesheet to make the color scheme look a bit more like visual studio.

And now I can post code like this nice c# one-liner that shows the name of every file in the current directory:


```csharp
using System;  
using System.IO;  
using System.Linq;  
  
namespace ListNames  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            (from file in Directory.GetFiles(Directory.GetCurrentDirectory(), "*", SearchOption.AllDirectories)   
             select file).ToList().ForEach(file => Console.WriteLine(file));  
            Console.ReadLine();  
        }  
    }  
}
```

A coworker of mine showed me a program that could do this, and I said well I can do that in one line - it is always nice to show off :) - (disclaimer:  I do not generally advocate to code in as few lines as possible. :) One issue you must always consider is readibility, though generally I must say that one of the great advantages of C#3.0/Linq syntax is that it tends to make stuff more readable when used correctly.

Speaking of Linq, as you can see the Syntaxhighlighter doesn't recognize Linq syntax. The "from" and "select" keywords are not highlighted (unless you read this when I have updated the shBrushCsharp.js script).



