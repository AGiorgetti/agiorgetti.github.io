---
layout: post
title: dotnet build - Microsoft.WebApplication.targets not found
comments: true
disqus_identifier: 62926053-bb79-4f48-9356-4e3ae1f501d1
tags: [dotnet]
---

Do you want to compile the old Website Projects (csproj / vbproj) file with the new "dotnet build" CLI command? 

In case of build process failure an error similar to the following one might have been reported:

```
Sample.Web.csproj(2620,3): error MSB4019: The imported project "C:\Program Files\dotnet\sdk\5.0.301\
Microsoft\VisualStudio\v16.0\WebApplications\Microsoft.WebApplication.targets" was not found.
Confirm that the path in the Import "C:\Program Files\dotnet\sdk\5.0.301\Microsoft\VisualStudio\v16.0\
WebApplications\Microsoft.WebApplication.targets" is correct, and that the file exists on disk.
```

Paths might change but the problem is the CLI is not able to properly locate the required `.targets` files (this can often happen on a build server).

The easy solution:

Add the MSBuild.Microsoft.VisualStudio.Web.targets NuGet package (a newer version can be used if available) to the Website project: 

{% highlight xml %}
<package id="MSBuild.Microsoft.VisualStudio.Web.targets" version="14.0.0.3" targetFramework="net48" />
{% endhighlight %}

The following line of code will be added to your .csproj file:

{% highlight xml %}
<Import Project="..\..\packages\MSBuild.Microsoft.VisualStudio.Web.targets.14.0.0.3\build\MSBuild.Microsoft.VisualStudio.Web.targets.props" Condition="Exists('..\..\packages\MSBuild.Microsoft.VisualStudio.Web.targets.14.0.0.3\build\MSBuild.Microsoft.VisualStudio.Web.targets.props')" />
{% endhighlight %}

The following code is not strictly needed, but if you want you can also find the following lines in your .csproj file:

{% highlight xml %}
  <PropertyGroup>
    <VisualStudioVersion Condition="'$(VisualStudioVersion)' == ''">10.0</VisualStudioVersion>
    <VSToolsPath Condition="'$(VSToolsPath)' == ''">$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)</VSToolsPath>
  </PropertyGroup>
  <Import Project="$(MSBuildBinPath)\Microsoft.CSharp.targets" />
  <Import Project="$(VSToolsPath)\WebApplications\Microsoft.WebApplication.targets" Condition="'$(VSToolsPath)' != ''" />
  <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\WebApplications\Microsoft.WebApplication.targets" Condition="false" />
  <Target Name="MvcBuildViews" AfterTargets="AfterBuild" Condition="'$(MvcBuildViews)'=='true'">
    <AspNetCompiler VirtualPath="temp" PhysicalPath="$(WebProjectOutputDir)" />
  </Target>
{% endhighlight %}

and change them like this:

{% highlight xml %}
  <PropertyGroup>
    <VisualStudioVersion Condition="'$(VisualStudioVersion)' == ''">10.0</VisualStudioVersion>
    <VSToolsPath Condition="'$(VSToolsPath)' == ''">..\..\Packages\MSBuild.Microsoft.VisualStudio.Web.targets.14.0.0.3\tools\VSToolsPath</VSToolsPath>
  </PropertyGroup>
  <Import Project="$(MSBuildBinPath)\Microsoft.CSharp.targets" />
  <Import Project="$(VSToolsPath)\WebApplications\Microsoft.WebApplication.targets" Condition="'$(VSToolsPath)' != ''" />
  <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\WebApplications\Microsoft.WebApplication.targets" Condition="false" />
  <Target Name="MvcBuildViews" AfterTargets="AfterBuild" Condition="'$(MvcBuildViews)'=='true'">
    <AspNetCompiler VirtualPath="temp" PhysicalPath="$(WebProjectOutputDir)" />
  </Target>
{% endhighlight %}

You can now compile old Websites .csproj / .vbproj using: `dotnet build`.

Have fun!