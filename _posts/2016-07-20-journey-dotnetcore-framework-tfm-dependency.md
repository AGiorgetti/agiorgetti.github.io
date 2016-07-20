---
layout: post
title: Journey to the .NET Core - Frameworks, TFM and Dependencies
comments: true
disqus_identifier: F78D08D7-DC1D-48BA-B2E5-1310034742F3
tags: [.NET Core, .NET]
---

When you deal with .NET projects you should keep the following concepts in mind:

- Framework: it's a stand alone implementation exposing a well-defined Runtime environment, an API and utility functions. Several versions of the .NET framework exist each or them adding new feature and breaking changes over the previous one.
- [Target Framework Moniker](https://docs.nuget.org/create/targetframeworks): it's a short name to identify a specific version of the framework.
- Dependency: with this term we typically intend a part of the features exposed by the framework (or by a library) and packaged together as a whole. The package is then made available with a package manager (like: [NuGet](https://www.nuget.org/)).

Let's start by creating an empty project using the Command Line tool, open up a console, create a new folder for the project and launch this command:

{% highlight bat %}
dotnet new
{% endhighlight %}

Now look at the project.json file that was created for us:

{% highlight json %}
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
{% endhighlight %}

This file will contain some project metadata and information on all the frameworks we would use to compile and run the application alongside with all the references to any dependency packages that are needed by the application to work.

The complete reference to this file can be found here: [project.json reference](https://docs.microsoft.com/it-it/dotnet/articles/core/tools/project-json) and here: [NuGet project.json format](https://docs.nuget.org/consume/projectjson-format)

The most important bits are:

__frameworks__: it's a json object that specifies the frameworks supported by this project, they must be valid [Target Framework Monikers](https://docs.nuget.org/create/targetframeworks); here we have selected "netcoreapp1.0" which means we are targeting the 1.0 release of .NET Core. To target multiple frameworks just add them above or under "netcoreapp1.0".

Some of the most commonly used frameworks (up to this date are):

- "netcoreapp1.0" For .NET Core 1.0.
- "net45", "net451", "net452", "net46", "net461" for .NET Framework versions.
- "portable-net45+win8" for Portable Class Library.
- "netstandard1.0", ... "netstandard1.6" for the [.NET Platform Standard](https://github.com/dotnet/corefx/blob/master/Documentation/architecture/net-platform-standard.md).

__dependencies__: it's a json object that's used to specify all the 'global' dependencies of the project. A dependency is usually identified by a name and a version tag. The packages specified here are meant to be available and used by all the different target frameworks (you should specify the minimum package version that can be used by all the different runtime). All the dependencies declared in this section refer to NuGet packages. If you need to use a package specifically designed for a particular framework you need to include it in the "dependencies" section of the framework object itself.

__frameworks/TFM/imports__: this setting is useful if you need to be able to reference and use packages compiled with previous versions of the framework (like preview or RC versions). "dnxcore50" in the example above refers to the preview releases of .NET Core.

__frameworks/TFM/dependencies__: this json object is used to specify all the dependencies that are specific to this version of the framework and that cannot be used by other runtime.

Let's use our NuGet package manager and 'restore' our dependencies, from the project root folder execute this command:

{% highlight bat %}
dotnet restore
{% endhighlight %}

This command will check your dependency tree, it will download any missing package from the configured NuGet feeds and in the end it will create a new file named 'project.lock.json'. This file will contain an exploded version of all the  packages used by the application, together with the information needed by the build and runtime system to find those files in global location where they are downloaded by NuGet.

To build and publish you project for the specified target framework you can issue the following commands:

{% highlight bat %}
dotnet build
dotnet publish
{% endhighlight %}

If you decide to support multiple frameworks you can use Visual Studio or the Command Line tool to decide for which of them compile and run the application.

Here's the same project.json file slightly modified to also support the .NET Framework 4.6 (the full framework):

{% highlight json %}
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
    },
    "net461" : {
      "dependencies": {
      }
    }
  }
}
{% endhighlight %}

When you execute the same build and publish commands saw before, the dotnet Command Line tool will compile a different version of the application for each framework you decided to support.

Here's how the folder structure will look like:

![Image]({{ site.url }}/UserFiles/images/dotnetcore-framework-tfm-dependency/01.PNG)

You can see specific folders for each target framework (they have the very same name) and the related nested publish folder.

If you want compile and publish just a single version of the application you can do that using the _-f_ switch from the command line specifying which TFM to use:

{% highlight bat %}
dotnet build -f net461
dotnet publish -f net461
{% endhighlight %}

This command will compile only the .NET Framewrok 4.6 version of the application.

For more information on the command line available options you can issue these commands

{% highlight bat %}
dotnet --help
dotnet restore --help
dotnet build --help
dotnet publish --help
{% endhighlight %}

Edit:
To select which version of the application to run, you can issue the following command from your project folder:

{% highlight bat %}
dotnet run -f net461
{% endhighlight %}

if you just type "dotnet run" it will pick the first framework of the list.

That's all for now!

__cya next__

Additional Resources:

- [project.json reference](https://docs.microsoft.com/it-it/dotnet/articles/core/tools/project-json)
- [NuGet project.json format](https://docs.nuget.org/consume/projectjson-format)
- [.NET Platform Standard](https://github.com/dotnet/corefx/blob/master/Documentation/architecture/net-platform-standard.md)
- [Running .NET Core apps on multiple frameworks and What the Target Framework Monikers (TFMs) are about](https://blogs.msdn.microsoft.com/cesardelatorre/2016/06/28/running-net-core-apps-on-multiple-frameworks-and-what-the-target-framework-monikers-tfms-are-about/)