---
layout: post
title:  "C# 6 new features (nameof operator)"
date:   2015-11-11 06:04:29 -0400
categories: C#
---
So C# 6 has been out for some time now but I haven't used it till recently when we switched to visual studio 2015 at work. I had read about all the features, but you can't know how awesome the are until you start using them.

#### [nameof](https://msdn.microsoft.com/en-us/library/dn986596.aspx) operator
This is what msdn says about it:
>Used to obtain the simple (unqualified) string name of a variable, type, or member.

Here's some simple examples
```csharp
nameof(Int32) //prints "Int32"
nameof(C) //prints "C"
nameof(customer.Name) //prints "Name"
```

Ok so that sounds good but where would this be used? Well I found out that there are many uses so lets get into some before and after examples.

##### vs 2013
```csharp
if(name == null) throw new ArgumentNullException("name");
```
Look familiar? Well theres a better way to do it now.
##### vs 2015
```csharp
if(name == null) throw new ArgumentNullException(nameof(name));
```
Now you don't have to worry about typos resulting in bugs like before. This is a simple example and very common one, but lets `continue;` with some more.

Imagine you have a class that implements `INotifyPropertyChanged`, and want to listen if the `Name` property has changed.
##### vs 2013 property changed event handler
```csharp
private void Test_PropertyChanged(object sender, PropertyChangedEventArgs e)
{
   if (e.PropertyName == "Name")
   {
       //do something  
   }
}
```
##### vs 2015 property changed event handler
```csharp
private void Test_PropertyChanged(object sender, PropertyChangedEventArgs e)
{
   if (e.PropertyName == nameof(Name))
   {
       //do something  
   }
}
```
It's a small change but again reduces bugs.

I've had fun introducing this into my code at work and if you haven't tried it yet you really need to.