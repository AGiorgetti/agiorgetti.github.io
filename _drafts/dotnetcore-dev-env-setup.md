---
layout: post
title: .NET Core - Windows Development Environment Setups
comments: true
disqus_identifier: GUID
tags: [.net, .netcore]
---

Your entry point to .NET Core should be this link:

[.NET Core](https://dotnet.github.io/)

Here you will find some very easy to follow tutorials on how to install it and get started on your machine using your OS of choice.

To start developing your application in .NET Core on a Windows machine you basically have two choices:

- The command line tools and your favorite editor - lightweight and useful for small projects and on systems does not have Visual Studio.
- Visual Studio 2015 (and up) - if you like to use an IDE loaded with tools and facilities.

__Command Line and Visual Studio Code__

If you opted for the Command Line tools and your favorite text editor; a good candidate is Visual Studio Code, given it is available on all major Operating Systems; here's what I recommend to install:

- [.NET Core SDK for Windows](https://go.microsoft.com/fwlink/?LinkID=809122)
- [Visual Studio Code](https://code.visualstudio.com/)

You can also add some Visual Studio Code extensions I find very useful (you can install them directly from Visual Studio Code using the "ext install _name_" command):

- [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [TSLint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint)
- [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
- [Powershell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) 

To start a new .NET Project using the Command Line and Visual Studio Code you will need to setup your solution structure almost all manually:

1. Create an empty "dotnetcore" folder, this will be our solution folder.
2. Inside that create an "src" folder, it will contain all our projects.
3. Create a "global.json" file, it will be used by the command line tools to gather metadata and configurations, this file should contain something like: {% highlight bat %}{ "projects": ["src", "test"] }{% endhighlight %}
4. Create another folder named "ConsoleApp" inside "src" and open up a Command Prompt at that location, launch these commands:

{% highlight bat %}
dotnet new
dotnet restore
dotnet build
dotnet run
{% endhighlight %}

A new empty .NET Core console project will be created, nuget packages will be restored, then the project will be compiled and executed. 

Two new files will be created for you:

- a "project.json" file: it will contains references to the framework version and any dependencies you'll later add to the project along with other metadata.
- a "program.cs" file that will act as the starting point of the application itself.

If you open up the folder in Visual Studio Code, it will automatically recognize this as a .NET Core project and it will create all the support files and settings needed by the editor to be able to build, debug and run the project. 

__Visual Studio 2015__

If you want to use Visual Studio to author your projects and to have a fully featured development and debugging experience you have to install the following packages:

- [Visual Studio 2015 Update 3](https://www.visualstudio.com/news/releasenotes/vs2015-update3-vs)
- [.NET Core for Visual Studio](https://go.microsoft.com/fwlink/?LinkId=817245)

These two packages will install all the editor extensions and templates needed to manage the new projects created for .NET Core.

To start a new .NET Core project using Visual Studio 2015:

1. Launch Visual Studio 2015
2. Select "File" -> "New Project"
3. Navigate to "Templates \ Visual C# \ .NET Core"

![Visual STudio 2015 new .NET Core project]({{ site.url }}/UserFiles/images/dotnetcore-dev-env-setup/01.PNG)

Select ".NET Core Console project", name it "DotNetCoreConsole" and hit "OK".
The complete solution structure will be created for you:

![Visual STudio 2015 - Solution Structure]({{ site.url }}/UserFiles/images/dotnetcore-dev-env-setup/02.PNG)

If you navigate there you will find some of the files we previously talked about (global.json and project.json) and some new file specifically designed to work with Visual Studio:

- DotNetCoreConsole.sln - the usual Visual Studio solution definition file.
- DotNetCoreConsole.xproj - this is the new project file that will contain Visual Studio specific settings when it comes to deal with .NET Core projects.

We will look at the content of "global.json" and "project.json" in more detail in a future post.

__cya next__