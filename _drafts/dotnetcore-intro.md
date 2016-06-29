---
layout: post
title: .NET Core - Introduction & Application Types
comments: true
disqus_identifier: GUID
tags: [.net, .netcore]
---

Your entry point to .NET Core should be this link:

[.NET Core](https://dotnet.github.io/)

Here you will find some very easy to follow tutorials on how to install it on your machine using your OS of choice.

I'm using Windows 10 and Visual Studio 2015 so I choose to install both the Visual Studio add-in and the command line toolkit, to make some experience using those low-level tools also.

If you open up the 'project' in Visual Studio Code, it will automatically recognize this as a .NET Core project and it will create all the support files and settings needed by the editor to be able to build, debug and run the project. 

An important concept when it comes to .NET Core is: Application Portability, in short where and how you are able to run your applications.

.NET Core has 2 types of applications:

- Portable applications.
- Self Contained Applications.

__Portable Applications__

This is the default type of application and it essentially means that in order to able to run it, you need to have .NET Core (the correct version) installed on the machine.

Let's at a typical project.json file:

-- goes here -- 

Any dependency marked with the property "type":"platform" will not be included in your publish folder, as .NET Core will assume that it will be already installed in the system and globally available.

__Self Contained Applications__

In this kind of application, everything, including the runtime, is packaged together; this means you will be able to run the application on any machine that runs an OS compatible with the one you used to build the application itself.

https://docs.microsoft.com/it-it/dotnet/articles/core/app-types


