---
layout: post
title: Journey to the .NET Core - Application Types
comments: true
disqus_identifier: 4515D239-F9C3-49DC-A8D6-EE9D02D1F710
tags: [.NET Core, .NET]
---

An important concept when it comes to .NET Core is: Application Portability, in short where and how we are able to run our applications.

.NET Core has 2 types of applications:

- Portable applications.
- Self Contained Applications.

To illustrate the difference between these two types of application let's start creating a "multiple projects solution" to use with Visual Studio Code:

- Create a new folder which will contain the project hierarchy
- Create a "global.json" file in the folder:

{% highlight json %}
{
    "projects": [
        "PortableApp", "SelfContainedApp"
    ]
}
{% endhighlight %}

This file will be used by 'dotnet.exe' to work on all the projects of the solution.

- Create two subfolders named: "PortableApp" and "SelfContainedApp" and initialize the new 'empty' applications executing these bash command in each folder: 

{% highlight bat %}
dotnet new
dotnet restore
{% endhighlight %}

To compile and publish the application you can issue:

{% highlight bat %}
dotnet build
dotnet publish
{% endhighlight %}

__Portable Applications__

This is the default type of application and it essentially means that to be able to run it, you need to have .NET Core installed on the machine.

Let's look at a typical project.json file:

```
{
  "version": "1.0.0-*",
  "buildOptions": {
    "debugType": "portable",
    "emitEntryPoint": true
  },
  "dependencies": {},
  "frameworks": {
    "netcoreapp1.0": {
      "dependencies": {
        "Microsoft.NETCore.App": {
          "type": "platform",
          "version": "1.0.0"
        }
      },
      "imports": "dnxcore50"
    }
  }
}
```

The complete reference to this file can be found here: [project.json reference](https://docs.microsoft.com/it-it/dotnet/articles/core/tools/project-json)

Let's look at what defines a Portable App project:
__frameworks__: it's a json object that specifies the frameworks supported by this project, they must be valid [Target Framework Moniker](https://docs.nuget.org/create/targetframeworks); here we have selected "netcoreapp1.0" which means we are targeting the 1.0 release of .NET Core. To target multiple frameworks just add them above or under "netcoreapp1.0".

__dependencies__: it's a json object that's used to specify all the 'global' dependencies of the project. The packages specified here are meant to be available and used by all the different target frameworks. All the dependecies declared in this file refer to NuGet packages. If you need to use a package specifically designed for a particular framework you need to include it in the "dependencies" section of the framework object itself.

__framework/imports__: this setting is useful if you need to be able to run software compiled with previous releases of the framework (like preview or RC versions). "dnxcore50" in the example above refers to the preview releases of .NET Core.

Any dependency marked with the property ```"type":"platform"``` will not be included in your publish folder, as .NET Core will assume that it will be already installed in the system and globally available.

__Self Contained Applications__

In this kind of application, everything, including the runtime, is packaged together; this means you will be able to run the application on any machine that runs an OS compatible with the one you used to build the application itself.

https://docs.microsoft.com/it-it/dotnet/articles/core/app-types

Additional Resources:
- [Running .NET Core apps on multiple frameworks and What the Target Framework Monikers (TFMs) are about](https://blogs.msdn.microsoft.com/cesardelatorre/2016/06/28/running-net-core-apps-on-multiple-frameworks-and-what-the-target-framework-monikers-tfms-are-about/)



