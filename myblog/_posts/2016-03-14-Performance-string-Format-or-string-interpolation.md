---
layout: post
title:  "Use String.Format or string interpolation in csharp"
date:   2016-03-14 06:04:29 -0400
categories: csharp
---
Recently I read the book Writing High-Performance.NET Code by Ben Watson.
The book gives so many helpful tips to making your code run amazingly fast!
Ben is part of the Bing team so he knows what takes to have fast efficient code.

One of the things he mentioned was that the `String.Format` method is expensive and should not be used unless really needed.
``` csharp
var message = string.Format("The job for {0} is finished", customerName);
```

Instead he recommends using concatenation like this
``` csharp
var message = "The job for " + customerName + " is finished";
```

So that got me thinking what about C# 6's new feature string interpolation? That allows you to write something like this.
``` csharp
var message = $"The job for {customerName} is finished";
```

So how does that perform? I decided to find out.

I wrote three different methods: one that used string format, one with string interpolation and one with string concatenation.

Here are the methods
``` csharp
        public static void UseStringFormat(int number)
        {
            var random = new Random();
            var text = "";
            for (var i = 0; i <= number; i++)
            {
                text = string.Format("First string {0} second string {1}", random.Next(i, i + 1000), i);
            }
        }

        public static void UseStringInterpolation(int number)
        {
            var random = new Random();
            var text = "";
            for (var i = 0; i <= number; i++)
            {
                text = $"First string {random.Next(i, i + 1000)} second string {i}";
            }
        }

        public static void UseStringConcat(int number)
        {
            var random = new Random();
            var text = "";
            for (var i = 0; i <= number; i++)
            {
                text = "First string " + random.Next(i , i + 1000) + " second string " + i;
            }
        }
```

And using the code looked like this

``` csharp
            var stopFormat = Stopwatch.StartNew();
            UseStringFormat(1000000);
            stopFormat.Stop();
            Console.WriteLine($"String format time: {stopFormat.Elapsed}");

            var stopInterpolation = Stopwatch.StartNew();
            UseStringInterpolation(1000000);
            stopInterpolation.Stop();
            Console.WriteLine($"String interpolation time: {stopInterpolation.Elapsed}");

            var stopConcat = Stopwatch.StartNew();
            UseStringConcat(1000000);
            stopConcat.Stop();
            Console.WriteLine($"String concatentation time: {stopConcat.Elapsed}");
```

I also called each of the methods before running the tests to make sure everything was jitted properly.

And the results.
![](https://raw.githubusercontent.com/jsweiler/StringPerformance/master/StringPerformanceConsole/times.PNG)

So it looks like string interpolation is just about the same as `string.format()` but using concatenation is significantly better.

And this is obviously not a very extensive test and your results may be different, so be sure to check your code before going ahead and changing things.

#####Conclusion
So we can see that the concatenation was the fastest method, but should you always use it now? Well it probably won't be necessary in many cases, but it is good to know what to use when performance really matters.
