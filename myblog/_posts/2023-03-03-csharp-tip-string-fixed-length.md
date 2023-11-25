---
layout: post
title:  "c# quick tip - string fixed length"
date:   2023-03-03 06:04:29 -0400
categories: csharp
---

I'm going to share a quick tip on how to easily create strings of a fixed length. For example suppose you have two strings like this.

```csharp
var text1 = "Short text";
var text2 = "This is a longer text";
```
But you want to have this instead:

"Short text................"
"This is a longer text....."
So you want all strings to be 25 characters long and fill in with . if needed to get there.

One way you could do this is like this:

`var newText2 = text2 + new string('.', 25 - text2.Length);`

That would give you This is a longer text.... for text2 and Short text............... for text1.

Now the second parameter in the string constructor cannot be negative so you want to check that maybe like this `new string('.', (25 - text2.Length) < 0 ? 0 : 25 - text2.Length)`.

However there is another way that is even easier. You can use `PadRight` to accomplish the same thing. So `var newText2 = text2.PadRight(25, '.');` will give you the same result This is a longer text....

There is also a `PadLeft` if you want to right align your text instead.

Hopefully this quick tip may be of value to you sometime.