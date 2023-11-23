---
layout: post
title:  "charp switch syntax overview"
date:   2023-03-06 06:04:29 -0400
categories: csharp
---

The C# switch statement can be a very handy way to avoid having many if/elseif statements. You probably have used the switch syntax many times in your C# code. Switch statements over the last few years have gotten some improvements which we will look at. You have probably seen code like this before:

```charp
public string BuildText(int stars)
{
    switch (stars)
    {
        case 1:
        case 2:
            return "This is a very poor rating.";
        case 3:
            return "This is a mediocre rating.";
        case 4:
        case 5:
            return "This is an acceptable rating.";
        default:
            return "This is either a really good, or really bad rating.";
    }
}
```
In C# 8 there was an update that would allow you to go like this instead.

```charp
return stars switch
{
    1 => "This is a very poor rating.",
    2 => "This is a very poor rating.",
    3 => "This is a medicre rating.",
    4 => "This is an acceptable rating.",
    5 => "This is an acceptable rating.",
    _ => "This is either a really good, or really bad rating."
};
```
This is a statement that will return a string. That is clean but we can make it even better. We can get rid of the duplicate code by using the or syntax:

```charp
return stars switch
{
    1 or 2 => "This is a very poor rating.",
    3 => "This is a medicre rating.",
    4 or 5 => "This is an acceptable rating.",
    _ => "This is either a really good, or really bad rating."
};
```
In C# 9 they introduced pattern matching in switch statements. Let's say you have a simple Person class like this.

```charp
public class Person
{
    public string Name { get; set; }
}
```
You could easily write a switch statement to only match on a Person with a Name of John.

```charp
var person = new Person { Name = "John" };
var text = person switch
{
    Person { Name: "John"} => "Hello John",
    _ => "I don't recognize you, sorry"
};
```
You could also write it like this, giving a variable name for the person and using a when clause to match on a specified condition.

```charp
var text = person switch
{
    Person per when per.Name is "John" => "Hello John",
    _ => "I don't recognize you, sorry"
};
```
I like the first person switch example myself, but both ways do work. I should also note that when using the property pattern you can match on more that just John by doing this:

```charp
var text = person switch
{
    Person { Name: "John" or "Sally"} => "Hello John or Sally",
    _ => "I don't recognize you, sorry"
};
```
You will have noticed I made use of the discard _ quite a bit. This is an alternative to the default option you would use in the more traditional C# syntax.

As you can see switch statements have gotten more powerful over the years and also much more concise. I certainly like the shorter syntax and also the ability to do pattern matching as well.