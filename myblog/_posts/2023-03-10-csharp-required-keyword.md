---
layout: post
title:  "charp switch syntax overview"
date:   2023-03-10 06:04:29 -0400
categories: csharp
---

The required keyword is a new feature in C# 11 and it can be very helpful to make sure your objects are used as they are intended to be. The name is fairly self explanatory, the required keyword allows you to specify that fields and properties must be passed in via an object initializer.

This keyword applies to classes, structs, records, and record structs.

You use it like this:

```csharp
public class Person
{
    public required string Name { get; set; }
    public required int Age { get; set; }
}
```
If you try to create a Person object without passing in any values, you will get a compiler error. You cannot do this: var person = new Person(). You need to specify the required values:

```csharp
var person = new Person
{
    Name = "Bob",
    Age = 5
};
```
I made both of my properties required, but I could have some that are and some that aren't.

This is much better than the previous way of making a class that had properties that were needed. Before you needed to specify a constructor and pass them in. It required more code to do the same thing:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }

}
```
I will note that you can use the SetsRequiredMembers attribute on a class and that will tell the compiler that the constructor initializes all required members, but it doesn't actually verify anything. However, I would use caution before using this.

I think this is a very nice feature and am excited to use it more often. It will be something else for new programmers to learn, but in this case I believe the name is very clear and descriptive of what the keyword is actually used for.

