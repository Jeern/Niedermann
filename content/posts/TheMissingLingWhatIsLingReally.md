---
title: "The Missing Linq / What is Linq really ?"
date: 2009-03-18T01:00:00+01:00
draft: false
aliases:
    - /2009/03/18/TheMissingLinqWhatIsLinqReally.aspx
---
What is Linq ? You probably think you know the answer, Linq is after all already quite dated. Well in my experience chances are you only know part of the story. Most of the people I have met have a slight misconception of Linq which limits their use of this great technology.

Let me explain. Linq of course means "Language Integrated Query", we all knew that. This basically refers to the syntactic sugar that Microsoft has poured over the C#3.0 language. I.e the from, where and select keywords and a few more.

In practice Linq is more. It is also a set of Extension methods in a given Linq provider, and a new way of coding, and furthermore many of the C#3.0 features are there to make Linq more useful. Like the var keyword, and lambda expressions.

What I think is limiting the use of Linq is that most people think Linq is a database technology. It is true that Linq can be used for querying databases if you use a Linq provider for a database technology like The LinqToEntities, LinqToSQL or Linq to some other ORM. This is basically Microsoft's "fault", Linq can be used for querying databases, and that is how Linq was promoted, I guess because of the ORM hype.

I was so fortunate to talk to one of the creators of Linq, Anders Hejlsberg in October 2008 when he was giving a lecture in Hilleroed, Denmark. I presented my view to him, that I thought it was a pity that Linq was viewed only as a database technology. He agreed with me, and said that this was because database access was the driving force behind Linq to begin with, and for marketing reasons.

But what is Linq really then ? If you look at an old fashioned SQL query

```sql
SELECT SomeColumn FROM SomeTable
```

You query relational data (SomeTable), and in return you get relational data (A set containing SomeColumn). With Linq you have much more power. You can query anything that implements the IEnumerable<XXX> interface, and you can return one or more .NET objects of any type.

So therefore I think it is much more productive to look at Linq as a means to transform data. In some ways Linq can replace XSLT, I am no XSLT expert but I would guess that you can always write a Linq expression to replace any XSLT transformation, as long as you work in a .NET assembly. Of course an XML document can refer directly to an XSLT transformation, which is not possible with Linq.

Transformations of data are so common in a computer program that we don't even notice them. In fact we use them all the time. And consequently we can use Linq expressions all the time, and should use them most of the time. I would argue that a Linq expression is more readable than "normal" code most of the time. If it is less readable or performs badly you might choose another option.

Here are a few examples of transformations that we don't think of as transformations:

Outputting errors to a logfile, or the console, or an eventlog. In this case exceptions are transformed to lines of text.
Building a dynamic menu from data in an XML file. In this case the data in the XML file is transformed into menuitems.
Building a dynamic Xaml page from some input. The input data is transformed into Xaml.
I will now give you some concrete codefragments to show these types of queries.

# Outputting errors

Let's say that you have collected error information in the form of a List of Exceptions. Assume in the following code that ErrorList is a `List<Exception>`:

```csharp
(from excep in ErrorList   
 where excep is ArgumentException  
 select excep).ToList()  
 .ForEach(e => Console.WriteLine(e.Message));
```

Now you have transformed your Exception data to Visual Data in the Console. In this example only ArgumentException's are printed. Of course you could easily print something better than just the message, e.g ToString() or perhaps a PrettyPrint() extension method for Exception that digs all InnerExceptions out, and prints everything in a nice readable format.

# Building a dynamic menu

This example is from my [WiiCursor](http://wiicursor.codeplex.com) project:

```csharp
m_ConfigurationMenu = new MenuItem("Configure");  
var confMenus =   
  from configuration in configurations  
  select Util.GetMenuItem(  
    configuration.Name,   
    (sender, args) => Configure((MenuItem)sender, configuration),   
    configuration.Default);  
m_ConfigurationMenu.MenuItems.AddRange(confMenus.ToArray()); 
```

In the code shown 'configurations' is a `List<WCConfiguration>` where WCConfiguration is a class that corresponds to an XML file, and gets initialized by the file. The GetMenuItem(...) method basically "news up" the MenuItem and connects a click eventhandler to it by passing the lambda expression '(sender, args) => Configure((MenuItem)sender, configuration)' to the MenuItems constructor.

So when the user clicks the MenuItem what happens is that the Configure(...) method is called with the MenuItem and most importantly the correct WCConfiguration as parameters.

This might not be the most readable code in the universe. In fact I violate one of my own best practices. Only use the var keyword if the resulting type, here MenuItem, is readable in another way. In the original version I called new MenuItem(...) directly, so that is my (admittedly bad) excuse.

But still the code beautifully illustrates how a Linq expression can Transform the context of XML files to MenuItems that act upon the information in the corresponding file.

# Building a dynamic Xaml page

In another project of mine, not yet published, I experiment with logging mouse moves to help me register the time I have used on different projects at the end of the week. A common task for employees around the world. I log the mousemoves in an XML file using a custom format.

```xml
<?xml version="1.0" encoding="utf-8"?>  
<Activities>  
  <Activity Name="Activity" Start="10-03-2009 08:01:42" End="10-03-2009 10:01:54" />  
  <Activity Name="Activity" Start="10-03-2009 12:00:00" End="10-03-2009 16:01:05" />  
</Activities>
```

To make it more appealing visually I transform it to a Xaml file that I load dynamically in a WPF project. The smart thing is that I can do it in a VB.NET assembly combining XML Literals and LinqToXML. Below is the first few lines of the Transformer class that contains the methods to create the Xaml file. Notice how the Xaml can be inlined easily in VB.NET since it is also XML.

```basic
Imports System.Linq
Imports System.Linq.Enumerable
Imports System.Xml.Linq
Imports <xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation">
Imports <xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

Public Class Transformer

    Private Const Column0Width As Integer = 50
    Private Const Column1Width As Integer = 50
    Private Const Column2Width As Integer = 70
    Private Const Column3Width As Integer = 170
    Private Const Column4Width As Integer = 40

    'Converts from a basic XDocument with Activities to a rich one using Xaml.
    'This is done with VB.NET because of the rich LinqToXML experience in VB.
    Public Shared Function ConvertToXaml(ByVal convertFrom As XDocument) As XDocument

        Dim months As Integer() = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12}


        Dim doc As XDocument = New XDocument( _
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    Title="Time Statistics" Height="302" Width="514">
    <Grid>
        <TabControl Name="ActivitiesTabControl">
            <%= From month In months _
                Select _
                <TabItem Header=<%= NameOfMonth(month) %> Height="20" Width="80" Selector.IsSelected=<%= SelectMonth(month) %>>
                    <ScrollViewer VerticalScrollBarVisibility="Auto">
                        <Grid>
                            <%= ColumnDefinitions() %>
                            <%= RowDefinitions(NumberOfRows(month, convertFrom)) %>
                            <%= RowHeaders() %>
                            <%= RowsOfMonth(month, convertFrom) %>
                        </Grid>
                    </ScrollViewer>
                </TabItem> %>
        </TabControl>
    </Grid>
</Window> _
                )
        Return doc
    End Function
    ...
```

XML Literals is the only reason why I sometimes use VB.NET, otherwise I am a C# guy. I would have loved C# to have this nice feature too, my guess is that it was impossible because angel brackets '<' are reserved for generics in C#.

Apart from giving an appealing visual look the code also makes calculations like summing up the time used in an entire day. Linq is ideal for this too.

# Summing up

I hope I have convinced you that Linq is more than just a database technology. You knew it already of course, but did you live it and breathe it :O) In my opinion Linq is such an integral part of C#3.0 that there is no avoiding using it all the time.
