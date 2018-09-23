---
title: "Best practice use of the var keyword in C#"
date: 2009-04-07T01:00:00+01:00
draft: false
aliases:
    - /2009/04/07/BestPracticeUseOfTheVarKeywordInC.aspx
---
I recently was in a company where I created the coding standard. Mostly by referring to the great standard from IDesign but with a twist of company specific things. When C#3.0 came out I couldn't find any coding standard material on the new stuff out there on the web. So I was forced to think...

My thoughts on the var keyword, is that it should be used extensively. But I place a restriction on it. You should only use it if the type of the object is explicitly readable from the line.

So this is allowed:

```csharp
var person = new Person();  
```

And this is not allowed:

```csharp
var person = PersonFactory.Create();
```

And similar for a Linq expression.

Allowed (actually it is required because of the anonymous type):

```csharp
var person =   
  from c in Customers  
  select new { c.FirstName, c.LastName };
```

Not allowed:

```csharp
var person =   
  from c in Customers  
  select Person.Create(c.FirstName, c.LastName);
```

Otherwise I place no restriction on the use. This way you get maximum readability without comprising that you should always know the type of what you are dealing with. Even when not using intellisense.

At the meeting I mentioned in "[The Missing Linq](/2009/03/18/the-missing-linq-/-what-is-linq-really-/)" I asked Anders Hejlsberg why var is not possible on member variables in C#. He told me that they are looking into parallelizing the compiler. And it would be impossible to infer the type across assemblies compiled by different threads of the compiler.