---
layout: post
title: dotnet build - Microsoft.WebApplication.targets not found
comments: true
disqus_identifier: GUID
tags: [TAGS]
---

Do you want to compile the old Website Projects (csproj / vbproj) file with the new "dotnet" CLI ? 

Sometimes you might fail and an error similar to the following one might have been reported:

 ...\Sample.Web.csproj(2620,3): error MSB4019: The imported project "C:\Program Files\dotnet\sdk\5.0.301\Microsoft\VisualStudio\v16.0\WebApplications\Microsoft.WebApplication.targets" was not found. Confirm that the path in the Import "C:\Program Files\dotnet\sdk\5.0.301\Microsoft\VisualStudio\v16.0\WebApplications\Microsoft.WebApplication.targets" is correct, and that the file exists on disk.

Paths might change but the problem is the CLI is not able to properly locate the required .targets files (this can often happen on a build server).

One of the possible solutions:

add the following NuGet package: 

<package id="MSBuild.Microsoft.VisualStudio.Web.targets" version="14.0.0.3" targetFramework="net48" />

this line of code will be added to your .csproj file:

<Import Project="..\..\packages\MSBuild.Microsoft.VisualStudio.Web.targets.14.0.0.3\build\MSBuild.Microsoft.VisualStudio.Web.targets.props" Condition="Exists('..\..\packages\MSBuild.Microsoft.VisualStudio.Web.targets.14.0.0.3\build\MSBuild.Microsoft.VisualStudio.Web.targets.props')" />

The following is not strictly needed but if you want you can also find the following code in your .csproj file:

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

and change it like:

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

you can now compile the old Website .csproj / .vbproj using: "dotnet build".

Have fun!














Backthicks:

```
text without markdown
```

```inline text without markdown```

Link:

[page](http://pygments.org/docs/lexers/)

Image:

![IncrediBuild Setup 5]({{ site.url }}/UserFiles/images/incredibuild-first-look/5.PNG)

Code:

{% highlight js %}
...
{% endhighlight %}

[lexers](http://pygments.org/docs/lexers/)

Raw (show jekyll markup):

{% raw %}
{% highlight languageTag %}
... your code goes here ...
{% endhighlight %}
{% endraw %}

Bold:

**bold**

Italic:
 _italic_