---
layout: post
title:  "Create Nuget Packages with VS 2017"
date:   2017-03-30 06:04:29 -0400
categories: c# nuget packages
---
#### Visual Studio 2017
If you haven't tried out [Visual Studio 2017](https://www.visualstudio.com/) yet you really should. It won't take long to install either, they totally redesigned the installer so if you have an SSD you should be ready in 10 minutes. 
#####Common Project System
Along with that there is a new project system for Visual Studio projects. This means the big long and ugly .csproj files you were used to can be greatly reduced down to just a handle of lines. 

This won't work for Windows Forms or WPF applications yet, but that will come sometime. [Here is an issue](https://github.com/dotnet/roslyn-project-system/issues/1272) you can follow if you are interested in support for those. This will work for .NET Core projects and also class libraries.

#### NuGet packages
So what does this have to do with NuGet packages? Well I am going to show you how to convert a class libary to the new .csproj format, and then you will see how easy it is to create a NuGet package with Visual Studio. 

#### Getting started
I started by creating a class library project in Visual Studio using .Net 4.6.2. The initial .csproj file looks like this.
```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="15.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <Import Project="$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props" Condition="Exists('$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props')" />
  <PropertyGroup>
    <Configuration Condition=" '$(Configuration)' == '' ">Debug</Configuration>
    <Platform Condition=" '$(Platform)' == '' ">AnyCPU</Platform>
    <ProjectGuid>{DB0C1B63-2227-46E2-A9A3-F8D8E81B2369}</ProjectGuid>
    <OutputType>Library</OutputType>
    <AppDesignerFolder>Properties</AppDesignerFolder>
    <RootNamespace>UrlDownloader</RootNamespace>
    <AssemblyName>UrlDownloader</AssemblyName>
    <TargetFrameworkVersion>v4.6.2</TargetFrameworkVersion>
    <FileAlignment>512</FileAlignment>
  </PropertyGroup>
  <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' ">
    <DebugSymbols>true</DebugSymbols>
    <DebugType>full</DebugType>
    <Optimize>false</Optimize>
    <OutputPath>bin\Debug\</OutputPath>
    <DefineConstants>DEBUG;TRACE</DefineConstants>
    <ErrorReport>prompt</ErrorReport>
    <WarningLevel>4</WarningLevel>
  </PropertyGroup>
  <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|AnyCPU' ">
    <DebugType>pdbonly</DebugType>
    <Optimize>true</Optimize>
    <OutputPath>bin\Release\</OutputPath>
    <DefineConstants>TRACE</DefineConstants>
    <ErrorReport>prompt</ErrorReport>
    <WarningLevel>4</WarningLevel>
  </PropertyGroup>
  <ItemGroup>
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
    <Reference Include="System.Xml" />
  </ItemGroup>
  <ItemGroup>
    <Compile Include="Url.cs" />
    <Compile Include="Properties\AssemblyInfo.cs" />
  </ItemGroup>
  <Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />
</Project>
```
Since there is only one class this is not very long compared to how some files get in large projects, but still with the new project system it can be made much shorter. When this is converted to the new format this is what it looks like.
```xml
<Project Sdk="Microsoft.NET.Sdk">
  
  <PropertyGroup>
    <TargetFramework>net462</TargetFramework>
    <AssemblyName>UrlDownloader</AssemblyName>
    <AssemblyVersion>1.0.0.0</AssemblyVersion>
    <Version>1.0.0</Version>
    <AssemblyTitle>UrlDownloader</AssemblyTitle>
  </PropertyGroup>

  <ItemGroup>
    <Reference Include="System.Net.Http" />
  </ItemGroup>
  
</Project>
```
I think you will agree that it is much nicer to look and much more readable. The Visual Studio solution explorer also looks a little different when using this new format.
![](https://www.jweiler.com/content/images/2017/03/Capture.PNG)

#### Conversion tip 
If you need some help converting your .csproj file [you should check this out](http://www.natemcmaster.com/blog/2017/03/09/vs2015-to-vs2017-upgrade/). There is a lot of helpful information there.

#### Creating the package
Now if you right click on the Project in Visual Studio and click Properties you will see there is a Package tab on the left.
![](https://www.jweiler.com/content/images/2017/03/package-1.PNG)
It fills in some of the information based on what I put in the .csproj file. To create a NuGet package all you have to do is check the "Generate NuGet package on build" box and build the project. Then in my bin/Debug folder I see the .nupkg file.
![](https://www.jweiler.com/content/images/2017/03/nupkg.PNG)
With that you could upload it to NuGet or create an internal NuGet feed to use.

####  Conclusion
So the project I created doesn't really do much, but hopefully it got the point across of how easy it is to create a NuGet package.

Have you used the new project system? What do you think of it?
