---
layout: post
title:  "csharp 6 new features (expression-bodied members)"
date:   2015-11-11 09:04:29 -0400
categories: C#
---
This is another small feature that I really like because I believe if used properly it can really clean up your code.

First lets look at how we would override `ToString()` in previous versions and now.
#### VS 2013
```csharp
public override string ToString()
{
    return "mystring";
}
```
#### VS 2015
```csharp
public override string ToString() => "mystring";
```
So using the lamba `=>` syntax we can shorten methods up to one line of code. I love using this especially on `ToString()` but also on other simple methods in my code as well.

Methods aren't the only place where you can use Expression-Bodied members, here's another example.
#### VS 2013
```csharp
public class Amounts
{
    public int Number1 {get; set;}
    public int Number2 {get; set;}

    public int Sum
    {
        get { return Number1 + Number2; }
    }
}
```
#### VS 2015
```csharp
public class Amounts
{
    public int Number1 {get; set;}
    public int Number2 {get; set;}

    public int Sum => Number1 + Number2;
}
```
So here again using the `=>` syntax you can define the body of the property getter on one line.

I really like this feature, and if you didn't try it out yet you should!
