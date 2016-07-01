---
layout: post
title: .NET Core - Application Types
comments: true
disqus_identifier: GUID
tags: [TAGS]
---

An important concept when it comes to .NET Core is: Application Portability, in short where and how you are able to run your applications.

.NET Core has 2 types of applications:

- Portable applications.
- Self Contained Applications.

To illustrate the difference between these two types of application let's start creating a "multiple project solution" to use with Visual Studio Code:

1. Create a new folder which will contain the project hierarchy
2. Create a "global.json" file in the folder:

        {
            "projects": [
                "PortableApp", "SelfContainedApp"
            ]
        } 

this file will be used by 'dotnet.exe' to work on all the projects of the solution.

3. Create two subfolders named: PortableApp and SelfContainedApp and initialize the new 'empty' applications following the steps you found in the documentation mentioned at top of the article.

__Portable Applications__

This is the default type of application and it essentially means that in order to able to run it, you need to have .NET Core (the correct version) installed on the machine.

Let's look at a typical project.json file:

-- goes here -- 

Any dependency marked with the property "type":"platform" will not be included in your publish folder, as .NET Core will assume that it will be already installed in the system and globally available.

__Self Contained Applications__

In this kind of application, everything, including the runtime, is packaged together; this means you will be able to run the application on any machine that runs an OS compatible with the one you used to build the application itself.

https://docs.microsoft.com/it-it/dotnet/articles/core/app-types


