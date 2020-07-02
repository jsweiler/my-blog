---
layout: post
title:  "Async and await in c#"
date:   2016-02-20 06:04:29 -0400
categories: c# async
---
So [async](https://msdn.microsoft.com/en-us/library/hh156513.aspx?f=255&MSPPError=-2147217396) and [await](https://msdn.microsoft.com/en-us/library/hh156528.aspx) are keywords that were introduced with the release of Visual Studio 2012 and C#5. They certainly make asynchronous programming in C# a lot easier but it is important to learn a few things about them before making every method in your codebase async.

#### Async
So starting with async msdn says this:
>Use the async modifier to specify that a method, lambda expression, or anonymous method is asynchronous.

This enables you to write a method with a signature like this
``` csharp
public async Task DoSomeWork()
```
When you are using the async keyword it allows you to "await" something. So using the previous method we could do this:
``` csharp
public async Task DoSomeWork()
{ 
    //Task.Delay creates a task that completes after a time delay
    await Task.Delay(2000);
}
```
In real life you would actually do some real work instead of use [Task.Delay](https://msdn.microsoft.com/en-us/library/hh194873(v=vs.110).aspx), but hopefully you get the idea. When you use these keywords the compiler does the hard work of making things asynchronous. Asynchrony is very useful for applications that access the UI thread, because all that activity usually shares one thread. 

For example say your application has a button that when clicked performs a bunch of calculations that takes a number of seconds. If you call a synchronous method you application will freeze and look like it stopped working, when actually it is just waiting to complete the method. This is something that is frustrating to users, and thankfully asynchrony can help. So if used an asynchronous method your application would stay responsive while the work is being done.

#### Example
Here's an example imagining we have a WPF app with a button click event handler that downloads something from the web.
``` csharp
// Mark the event handler with async so you can use await in it.
private async void Button_Click(object sender, RoutedEventArgs e)
{
    var content = await AccessTheWebAsync();
    resultsTextBox.Text = content;
}

public async Task<string> AccessTheWebAsync()
{
    var httpClient = new HttpClient();
    var content = await httpClient.GetStringAsync("http://msdn.microsoft.com");
    return content;
}
```
So that's an example of how async and await can be used. The `AccessTheWebAsync` method doesn't take very long to complete but notice how the application is responsive during the time the method is running.

If you were to debug the code you would see that the method does wait till return till we have the result, but the UI thread is not being blocked in any way. This gives the user the freedom to do whatever they want while a task is being processed. So it does allow re-entry into the event handler because the user can press the button as many times as they please.

This could cause some problems, say if you were using an Entity Framework `DbContext` you would get an exception because the `DbContext` class is not threadsafe.

Here is an example but showing you the normal synchronous way.
``` csharp
private void SyncButton_Click(object sender, RoutedEventArgs e)
{
    resultsTextBox.Text = "";
    var content = AccessTheWeb();
    resultsTextBox.Text = content;
}

public string AccessTheWeb()
{
    var httpClient = new WebClient();
    var content = httpClient.DownloadString("http://msdn.microsoft.com");
    return content;
}
```
So in that example you will notice how that while we are waiting for the `AccessTheWeb` method to return the application is not responsive.

I've made a sample application on GitHub [here](https://github.com/jsweiler/AsyncAwaitBlog1). It doesn't do much but you can use that to see for yourself how the code works.

So this post gives you some info about async and await, and shows you a little how they can be used. Coming up soon we will dig deeper and look at what the compiler needs to do to make this all work great.

---
###### Sources
- [Msdn](https://msdn.microsoft.com/en-us/library/hh156513.aspx)
