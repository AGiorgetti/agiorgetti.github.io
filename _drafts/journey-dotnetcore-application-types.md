---
layout: post
title: Journey to the .NET Core - Application Types & Distribution Strategies
comments: true
disqus_identifier: 4515D239-F9C3-49DC-A8D6-EE9D02D1F710
tags: [.NET Core, .NET]
---

An important concept when it comes to .NET Core is Application Portability, in short where and how we are able to distribute and run our applications.

.NET Core has two types of applications:

- Portable applications.
- Self Contained Applications.

To illustrate the difference between these two let's start creating a "multiple projects solution" to use with Visual Studio Code:

- Create a new folder which will contain the project hierarchy.
- Create a "global.json" file in the folder with this content:

{% highlight json %}
{
    "projects": [
        "PortableApp", "SelfContainedApp"
    ]
}
{% endhighlight %}

This file will be used by the [dotnet utility](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/dotnet) to work on all the projects of the solution.

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

This is the default type of application and it essentially means that to be able to run it __you need to have .NET Core installed on the machine__.

A portable application is a .dll file that can be launched using the [dotnet tool](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/dotnet).

Let's look at a typical project.json file:

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

You can check my previous post [Journey to the .NET Core - Frameworks, TFM and Dependencies](http://www.primordialcode.com/blog/post/journey-dotnetcore-framework-tfm-dependency) to have some detailed information on the project.json file format.

Look at the _dependecies_ section: any dependency marked with the property ```"type":"platform"``` will not be included in your publish folder, as .NET Core tooling will assume that the runtime (or any other marked library) will be already installed in the system and globally available.

You can only have one dependency marked with the ```"type":"platform"``` attribute.

It's important to launch 'dotnet restore' every time we change the dependencies in the project.json file to let NuGet download and install any missing package, otherwise the build could fail.

If you build and publish the application and then look at the files that are generated you will see something like this:

![Portable Application]({{ site.url }}/UserFiles/images/dotnetcore-application-types/01.PNG)

None of the files included in the referenced package Microsoft.NETCore.App are present, they are all part of the .NET Core library and are supposed to be generally available on the target machine.

Once the application is built and published you can deploy that to any machine that has the .NET Core framework installed and execute it using this command from the console:

{% highlight bat %}
dotnet PortableApp.dll
{% endhighlight %}

If a Portable application has some native dependencies to other libraries, it will be as portable as all of its dependencies are portable; that means that you'll be able to run the application on any platform that those dependencies can run on.

__Self Contained Applications__

In this kind of application everything, including the runtime, is packaged together; this means you will be able to run it on any machine that runs an OS compatible with the one you used to build the application itself.

You'll need to make it _explicit_ which _platforms_ you're going to build the application for.

Switch over to the "SelfContainedApp" folder project; to create a Self Contained application you need to:

- Remove the ```"type":"platform"``` attribute off any dependency in the project.json file.
- Add a ```runtimes``` node in the project.json that will list all the [Runtime Identifiers / RIDs](https://docs.microsoft.com/it-it/dotnet/articles/core/rid-catalog) of the platforms to support.

Here's a modified project.json file that will produce a Self Contained Application for a Windows 10 and for a Linux Ubuntu machines:

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
          "version": "1.0.0"
        }
      },
      "imports": "dnxcore50"
    }
  },
  "runtimes": {
    "win10-x64": {},
    "ubuntu.14.04-x64": {}
  }
}
{% endhighlight %}

If you now issue the commands:

{% highlight json %}
dotnet restore
dotnet build
dotnet publish
{% endhighlight %}

It will compile a version of the application that is compatible with the system you are using. To build and publish a specific platform you need to specify which RID to build with the _--runtime_ switch:

{% highlight json %}
dotnet restore
dotnet build --runtime ubuntu.14.04-x64
dotnet publish --runtime ubuntu.14.04-x64
{% endhighlight %}

__Application Deploy__

Chosing one application type over the other will impact how you distribute the application.

At its core deploying the application consists in just copying the files in the 'publish' folder to the target machines.

Portable applications will require the target machines (or containers) to have the .NET Core framework already installed, so you need to configure them or use preconfigured images for those machines. Self Contained applications do not require that, because they carry everything over with them, but they need to run on the very specific OS there compiled for.

There's of course a difference in the size of the applications and you'll need to consider if that can impact your distribution transport strategy.

__cya next__