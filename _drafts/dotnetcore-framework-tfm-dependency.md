---
layout: post
title: .NET Core - Frameworks, TFMs and Dependencies
comments: true
disqus_identifier: GUID
tags: [.NET Core]
---

When you deal with .NET projects you must have clear the following concepts:

- Framework: it's a stand alone implementation exposing a well-defined API and utility functions. Several versions of the .NET framework exist each or them adding new feature and breaking changes over the previous one.
- Target Framework Moniker: it's a short name to identify a specific version of the framework. 
- Dependency: with the term dependency we typically identify a small portion of the features exposed by the framework (or by a library) and packaged together as a whole. The package is then made available with a package manager (like: NuGet).

Let's start by creating an empty project using the Command Line tool, open up a console and launch these commands:

{% highlight bat %}
dotnet new
dotnet restore
{% endhighlight %}

Let's look at the project.json file that was created for us:

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

To specify the framework(s) we would like to use the important bits are:

__frameworks__: it's a json object that specifies the frameworks supported by this project, they must be valid [Target Framework Moniker](https://docs.nuget.org/create/targetframeworks); here we have selected "netcoreapp1.0" which means we are targeting the 1.0 release of .NET Core. To target multiple frameworks just add them above or under "netcoreapp1.0".

__dependencies__: it's a json object that's used to specify all the 'global' dependencies of the project. The packages specified here are meant to be available and used by all the different target frameworks. All the dependecies declared in this file refer to NuGet packages. If you need to use a package specifically designed for a particular framework you need to include it in the "dependencies" section of the framework object itself.

__framework/imports__: this setting is useful if you need to be able to run software compiled with previous releases of the framework (like preview or RC versions). "dnxcore50" in the example above refers to the preview releases of .NET Core.

Any dependency marked with the property ```"type":"platform"``` will not be included in your publish folder, as .NET Core will assume that it will be already installed in the system and globally available.

__Self Contained Applications__

In this kind of application, everything, including the runtime, is packaged together; this means you will be able to run the application on any machine that runs an OS compatible with the one you used to build the application itself.

https://docs.microsoft.com/it-it/dotnet/articles/core/app-types

Additional Resources:
- [Running .NET Core apps on multiple frameworks and What the Target Framework Monikers (TFMs) are about](https://blogs.msdn.microsoft.com/cesardelatorre/2016/06/28/running-net-core-apps-on-multiple-frameworks-and-what-the-target-framework-monikers-tfms-are-about/)



