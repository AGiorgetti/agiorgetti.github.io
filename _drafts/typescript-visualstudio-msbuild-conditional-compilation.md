---
layout: post
title: TypeScript - Visual Studio / MSBuild conditional compilation
comments: true
disqus_identifier: 590D012E-6629-4734-82DA-1F92CB9DB6FE
tags: [typescript, msbuild, visual studio]
---

Conditional Compilation is not yet available in TypeScript, some requests and proposals have been made:

- [Support conditional compilation](https://github.com/Microsoft/TypeScript/issues/449)
- [Proposal: Conditional Compilation](https://github.com/Microsoft/TypeScript/issues/3538)
- [Proposal: Preprocessor Directives](https://github.com/Microsoft/TypeScript/issues/4691)

If you are using Visual Studio and MSBuild there's however a very simple workaround to completely exclude some TypeScript files from being compiled.

The solution is to use [MSBuild Conditions](https://msdn.microsoft.com/en-us/library/7szfhaft.aspx) and/or [MSBuild Conditional Constructs](https://msdn.microsoft.com/en-us/library/ms164307.aspx), it all depends on how complex your conditional logic will be.

In the simplest case you just have to open your .csproj (or .vbproj) file in a text editor and add the proper **Condition** attribute to the TypeScriptCompile entry; something like:

{% highlight xml %}
...
<TypeScriptCompile Condition="'$(CONFIG)'=='DEBUG'" Include="UiComponentFeature\UiComponent.ts" />;
...
{% endhighlight %}

With code like this we will compile that TypeScript file only if the DEBUG symbol has been defined for the current build configuration; that's all we really need to do!

_cya next_