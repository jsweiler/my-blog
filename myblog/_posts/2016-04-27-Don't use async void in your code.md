#### Async Code
So you probably know about [async](https://msdn.microsoft.com/en-us/library/hh156513.aspx) and [await](https://msdn.microsoft.com/en-us/library/hh156528.aspx) the keywords that C# has for quite some time. If you've used them no doubt you love how easy it is to write asynchronous applications, which is true. 

I'm going to share with a little gotcha that if you don't watch out for can literally bring your application crashing to the ground!

So lets' get into some code. Consider the following method:
``` csharp
async void DoWork()
{
    await Task.Delay(100);
    throw new Exception("bad code");
}

```
The method has the async keyword, which allows us to await inside it, however it also throws an exception which we would want to catch.

Following is a button click event in WPF that calls the method.
``` csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    try
    {
        DoWork();
    }
    catch (Exception)
    {
       //it nevers gets caught!!    
    }
}
```

As you can see by my comment and if you run this the Exception never gets caught instead it crashes the application! Not good, but why you might be wondering.

Stephen Cleary wrote a great article [here](https://msdn.microsoft.com/en-us/magazine/jj991977.aspx) describing why you shouldn't do this. I'll quote a little of what he says:
>When an exception is thrown out of an async Task or async Task<T> method, that exception is captured and placed on the Task object. With async void methods, there is no Task object, so any exceptions thrown out of an async void method will be raised directly on the SynchronizationContext that was active when the async void method started

So that is why. No his quote also tells us what we should do instead to the code to make it work. If we modify the method to return a Task instead everything will work fine.
``` csharp
async Task DoWorkAsync()
{
    await Task.Delay(100);
    throw new Exception("exception");
}
```
Now the click event will look like this
``` csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    try
    {
        await DoWorkAsync();
    }
    catch (Exception)
    {
       //it now gets caught.  
    }
}
```
Since it returns task we can await the method in the event handler.

But wait!! Now there is another async void in our code, in the event handler.
Well it turns out this is what async void was designed for. Async void cannot be awaited and since event handler methods aren't awaited it works great. 

Another thing to watch out for is passing an async lambda to a method that calls for an Action. This leaves you with the same problem. The solution would be to use async lambda for a method that returns a `Task`

Have you ran into this in your code? Also feel free to point out any issues with my explanation, as I am new 