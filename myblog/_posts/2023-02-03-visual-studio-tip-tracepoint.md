---
layout: post
title:  "visual studio tip - tracepoint"
date:   2023-02-03 06:04:29 -0400
categories: visualstudio
---

Today is a short post with a Visual Studio feature that can come in really handle when debugging code.
I'm sure you have used Breakpoints many times in Visual Studio

![](/assets/images/2023-02-03-tracepoint1.png)

Tracepoints are somewhat similar but also different. To create one right click on the left side of your Visual Studio window (where you would usually left click to create a breakpoint).

![](/assets/images/2023-02-03-tracepoint2.png)

That will give you a window like below.

![](/assets/images/2023-02-03-tracepoint3.png)

Then when you run your application you will see a line in the Output window everytime the tracepoint is hit.

![](/assets/images/2023-02-03-tracepoint4.png)

This example may be a bit simple, but if you can imagine an application where you are debugging various keyboard input for example this feature can make it very easy to see what is happening with out needing to stop program execution to inspect variables.