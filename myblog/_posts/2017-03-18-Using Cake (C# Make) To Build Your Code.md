---
layout: post
title:  "Using Cake (C# Make) To Build Your Code"
date:   2017-03-18 06:04:29 -0400
categories: cake c# make
---
#### What is Cake?
This is what their [website says](http://cakebuild.net/).
> Cake (C# Make) is a cross platform build automation system with a C# DSL to do things like compiling code, copy files/folders, running unit tests, compress files and build NuGet packages.

So really you can do much more than just build your code, you even can use Cake to create an installer for you. I really like that I can run a build on my local machine and it will be just like what happens on the build server.
#### Open Source
Cake is open source and you can [check it out here](https://github.com/cake-build/cake).
#### Getting Started
You need a build.cake file and a powershell script. [Here is an example](https://github.com/cake-build/example) where you can get both of those files. You should take a look at the sample and run the build.ps1 file for yourself to get an idea of how cake works. When you run it it will build and test the sample project.
#### Example
I added a cake script to [this project on github](https://github.com/jsweiler/CognitiveServicesAzure). This is a very simple project so my cake script is small and easy to understand.
```csharp
//////////////////////////////////////////////////////////////////////
// ARGUMENTS
//////////////////////////////////////////////////////////////////////

var target = Argument("target", "Default");
var configuration = Argument("configuration", "Release");

//////////////////////////////////////////////////////////////////////
// PREPARATION
//////////////////////////////////////////////////////////////////////

// Define directories.
var buildDir = Directory("CognitiveServicesAzure/bin") + Directory(configuration);

//////////////////////////////////////////////////////////////////////
// TASKS
//////////////////////////////////////////////////////////////////////

Task("Clean")
    .Does(() =>
{
    CleanDirectory(buildDir);
});

Task("Build")
    .IsDependentOn("Clean")
    .Does(() =>
{
    // Use MSBuild
    MSBuild("CognitiveServicesAzure.sln", settings => settings.SetConfiguration(configuration));
});


//////////////////////////////////////////////////////////////////////
// TASK TARGETS
//////////////////////////////////////////////////////////////////////

Task("Default")
    .IsDependentOn("Build");

//////////////////////////////////////////////////////////////////////
// EXECUTION
//////////////////////////////////////////////////////////////////////

RunTarget(target);

```
When running the build.ps1 on my computer I get the following result.
![](https://www.jweiler.com/content/images/2017/03/powershell.PNG)

This is pretty simple and most likely you would have more advanced things to to. For example if you have Nuget packages to restore you can add a restore step in the cake file like this.
```csharp
Task("Restore-NuGet-Packages")
    .IsDependentOn("Clean")
    .Does(() =>
{
    NuGetRestore("CognitiveServicesAzure.sln");
});
```
**Note about the Nuget version.**
The build.ps1 file loads the current version of Nuget from here https://dist.nuget.org/win-x86-commandline/latest/nuget.exe. As of 3/18/2017 that is still version 3.5.0. If you need to 4.0 version of nuget.exe you will need to edit the build.ps1 file and replace the url with the 4.0 one.

So if you like C# Cake is a very inviting option to use to build, test and release your project.

Have you used Cake? What do you think of it?