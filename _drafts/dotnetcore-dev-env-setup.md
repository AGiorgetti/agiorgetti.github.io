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

- Visual Studio 2015 (and up) - if you like to use an IDE loaded with tools and facilities.
- The command line tools and your favorite editor - lightweight and useful for small projects and on systems does not have Visual Studio.

__Visual Studio 2015__

If you want to use Visual Studio to author your projects and to have a fully featured development and debugging experience you have to install the following packages:

- [Visual Studio 2015 Update 3](https://www.visualstudio.com/news/releasenotes/vs2015-update3-vs)
- [.NET Core for Visual Studio](https://go.microsoft.com/fwlink/?LinkId=817245)

These two packages will install all the editor extensions and templates needed to manage the new projects created for .NET Core.

__Command Line and Visual Studio Code__

If you opted for the Command Line tools and your favorite text editor; a good candidate is Visual Studio Code, given it is available on all major OS; here's what I recommend to install:

- [.NET Core SDK for Windows](https://go.microsoft.com/fwlink/?LinkID=809122)
- [Visual Studio Code](https://code.visualstudio.com/)

You can also add some Visual Studio Code extensions I find very useful (you can install them directly from Visual Studio Code using the "ext install _name_" command):

- [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [TSLint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint)
- [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
- [Powershell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) 

If you open up the folder in Visual Studio Code, it will automatically recognize this as a .NET Core project and it will create all the support files and settings needed by the editor to be able to build, debug and run the project. 

