---
layout: post
title:  "c# overview of records"
date:   2023-03-06 06:04:29 -0400
categories: csharp
---

So if you are familiar with C# have interacted with many classes and most likely created many of your own classes.

A class is an object which can have things like properties and methods and much more on them.

Records were introduced in C# 9 as a keyword to define a [reference type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/reference-types) that provides built-in functionality for encapsulating data.

You can create a record in these two ways:

```csharp
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public int Age { get; init; }
};

public record Person(string FirstName, string LastName, int Age);
```

You will notice the first way looks very similar to a class with a few differences. One of them is the init keyword instead of a set. This was also introduced in C# 9 and this is what the Microsoft docs say:

An init-only setter assigns a value to the property or the indexer element only during object construction. This enforces immutability, so that once the object is initialized, it can't be changed again.

So if you try to set a property after creating the object like this you will get a compiler error:

```csharp
var person = new Person();
person.FirstName = "Bob";
```

You only can set the properties from the object initializer like this

```csharp
var person = new Person
{
    FirstName = "Bob",
    LastName = "Jones",
    Age = 30
};
```
In C#11 the required keyword was introduced and I show how you can apply that to a record. The required keyword when used on a property requires that you provide a value for it when you create the object. So we can change our record to look like this:

```csharp
public record Person
{
    public required string FirstName { get; init; }
    public required string LastName { get; init; }
    public required int Age { get; init; }
}
```
That forces us to pass in values in the object initializer or else we get a compiler error.

The second way of creating records looks like this:

`public record Person(string FirstName, string LastName, int Age);`
That is much simpler but what is it doing? Behind the scenes it creates a public auto-implemented property with an init setter for each parameter we have. These records are called positional records. We can create an instance of this record just like you would with a class:

`var person = new Person("Bob", "Jones", 30);`
In this way you are also forced to pass in all the parameters. Now if you wanted to use a positional record but also able to mutate one of the properties you could do something like this which would allow you to set Age later.

```csharp
public record Person(string FirstName, string LastName, int Age)
{
    public int Age { get; set; } = Age;
}
```
I personally like the short and easy syntax of the positional record. If you have an object that you want to be immutable, it's a very easy way to accomplish that.

One other thing to note about records is that unlike classes two records are equal to each other if they have all the same values and are of the same type. For example:

```csharp
var person = new Person("Bob", "Jones", 30);
var person2 = new Person("Bob", "Jones", 30);
var equal = person == person2;
// equal is true
```
Also there is a way to copy a record while changing some of the data using the with keyword. So you could go like this:

```csharp
var person = new Person("Bob", "Jones", 30);
var person2 = person with { FirstName = "Sally" };
```
That would give you a new record with the FirstName of Sally instead of Bob, the other properties would be the same. Also when doing this person == person2 would be false.

There is a lot more to records that what I mentioned here. If you want to learn more be sure to check out the [Microsoft docs site](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record).