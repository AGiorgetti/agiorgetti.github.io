---
layout: post
title: The referenced project is targeted to a different framework family
comments: true
disqus_identifier: b405739e-8db1-4502-8ed3-ae1d8c8a4f66
tags: [dotnet,multitarget,csproj,tips,troubleshooting]
---

The project 'Acme.Core' cannot be referenced. The referenced project is targeted to a different framework family (.NETCoreApp)

In a multitarget solution that also contains an old .Net Full Framework Web Project sometimes we might face that error.

The scenario is:

- A solution with multiple multitarget Library Projects and an Old Web Project.
- Acme.Core: a multitarget Library Project with the following target frameworks: net6.0, net5.0, netstandard2.0, net472
- Acme.Web: an old net472 Asp.Net MVC Web Project referencing Acme.Core.

If it happens that you get the above mentioned error, and you see a yellow warning mark in Visual Studio reference node tree of Acme.Web project, the problem might be the order of the target frameworks in the Acme.Core.csproj file.

The original .csproj file that caused the warning was like:

{% highlight xml %}
  <PropertyGroup>
    <TargetFrameworks>net6.0;net5.0;netstandard2.0;net472</TargetFrameworks>
  </PropertyGroup>
{% endhighlight %}

To solve the problem just change the order of target frameworks:

{% highlight xml %}
  <PropertyGroup>
    <TargetFrameworks>net472;net6.0;net5.0;netstandard2.0</TargetFrameworks>
  </PropertyGroup>
{% endhighlight %}

The first of the list is the one used by old Web Projects when making project references.

The hint to figure out the thing was in the warning message itself: `The referenced project is targeted to a different framework family (.NETCoreApp)'`

Old Web Projects that target full frameworks belongs to another framework family: `(.NETFramework)`.

For more informations on multi targeting and cross platform targeting head to the official docs:

- [Target frameworks in SDK-style projects](https://docs.microsoft.com/en-us/dotnet/standard/frameworks)
- [Cross-platform targeting](https://docs.microsoft.com/en-us/dotnet/standard/library-guidance/cross-platform-targeting)

_cya next_