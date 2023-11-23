---
layout: post
title:  "trying out avalonia"
date:   2023-03-01 06:04:29 -0400
categories: azure
---

###A simple overview of what Avalonia is.

I've heard about [Avalonia](https://avaloniaui.net/) recently and thought I want to write about about trying it out. It allow you to create Multi-Platform Apps with .NET using xaml. Xaml is one of the things I really enjoyed while working on WPF applications so that certainly sounds interesting to me.

They have a helpful [getting started page](https://docs.avaloniaui.net/docs/getting-started) to quickly get a sample running. To get started you will want to run the following command to install the templates:

`dotnet new install Avalonia.Templates`
After doing that if you go to Visual Studio and create a new project you should see options for creating an Avalonia project:

![](/assets/images/2023-03-01-avalonia1.png)


If you select the .NET Core App you will get a simple application with a MainWindow.axaml file. (.axaml is the extension for Avalonia views).

```xaml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaApplication1.MainWindow"
        Title="AvaloniaApplication1">
    Welcome to Avalonia!
</Window>
```
If you are familiar with WPF at all you will notice this is very much the same.

In the Program class you will see things look a little different than a typical WPF application.

```csharp
internal class Program
{
    // Initialization code. Don't use any Avalonia, third-party APIs or any
    // SynchronizationContext-reliant code before AppMain is called: things aren't initialized
    // yet and stuff might break.
    [STAThread]
    public static void Main(string[] args) => BuildAvaloniaApp()
        .StartWithClassicDesktopLifetime(args);

    // Avalonia configuration, don't remove; also used by visual designer.
    public static AppBuilder BuildAvaloniaApp()
        => AppBuilder.Configure<App>()
            .UsePlatformDetect()
            .LogToTrace();
}
```
When you run the application you should see a window show with the "Welcome to Avalonia!" text appear in it.

If you were looking at their docs you may have noticed there also is webassembly support which allows your app to run in the browser as well! This is not ready for production yet, but if you want to check that out see [this docs page](https://docs.avaloniaui.net/tutorials/running-in-the-browser).

If you go back and create an application using their MVVM template you will see something more familiar if MVVM is what you typically use.

![](/assets/images/2023-03-01-avalonia2.png)

This is just a simple overview but I find the project very interesting. While many people may not like the idea of using xaml to create user interfaces, to me it is appealing and makes Avalonia attractive to me.

They do have a [list of projects](https://docs.avaloniaui.net/misc/projects-that-are-using-avalonia) that use Avalonia so be sure to check them out. Also if you use Jetbrains Rider Avalonia is nicely [integrated into there](https://docs.avaloniaui.net/docs/getting-started/ide-support/jetbrains-rider-setup) as well.