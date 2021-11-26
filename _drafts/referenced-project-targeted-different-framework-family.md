---
layout: post
title: The referenced project is targeted to a different framework family
comments: true
disqus_identifier: b405739e-8db1-4502-8ed3-ae1d8c8a4f66
tags: [dotnet,multitarget]
---

The project 'Acme.BusinessLogic' cannot be referenced. The referenced project is targeted to a different framework family (.NETCoreApp)

In a multitarget solution that also contains an old .Net Full Framework Web Project sometimes we might face that error.

The scenario is:

- A solution with multiple multitarget library projects and an Old Web Project.
- Acme.BusinessLogic: a multitarget library project with the following target frameworks: net6.0, net5.0, netstandard2.0, net472
- Acme.Web: an old net472 Asp.Net MVC Web Project referencing Acme.BusinessLogic.

If it happens that you get the above mentioned error, and you see a yellow warning mark in the Visual Studio reference node tree of Acme.Web project, the problem might be the order of the target frameworks in the Acme.BusinessLogic.csproj file.

The original .csproj file that caused the warning was like:

{% highlight xml %}
  <PropertyGroup>
    <TargetFrameworks>net6.0;net5.0;netstandard2.0;net472</TargetFrameworks>
    <GeneratePackageOnBuild>false</GeneratePackageOnBuild>
  </PropertyGroup>
{% endhighlight %}

To solve the project just change the order of the target frameworks:

{% highlight xml %}
  <PropertyGroup>
    <TargetFrameworks>net472;net6.0;net5.0;netstandard2.0</TargetFrameworks>
    <GeneratePackageOnBuild>false</GeneratePackageOnBuild>
  </PropertyGroup>
{% endhighlight %}

The first of the list is the one used by old Web Projects when making project references.

The hint to figure out the thing was in the warning message itself: `The referenced project is targeted to a different framework family (.NETCoreApp)'`

Old Web Projects that target full frameworks belongs to another framework family: `(.NETFramework)`.

_cya next_