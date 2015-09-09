---
layout: post
title: TypeScript command-line Compiler
comments: true
tags: [typescript]
---

Let's have another introductory post on TypeScript. This time it's the command-line Compiler turn.

If you use a fully featured IDE with TypeScript support - like Visual Studio 2013+ - you will probably do not have to worry about the command-line compiler;
it's nonetheless usefull to have a grasp on some of its basic functionalities, 
it can come in handy when you do not have an IDE available or just to automate some tasks with a task runner.

**Compile a single file:**

{% highlight bat %}
tsc app.ts
{% endhighlight %}

generates app.js.

**compile multiple files**

{% highlight bat %}
tsc file1.ts file2.ts, file 3.ts
{% endhighlight %}

Multiple .js files will be created.

Wildcards are not supported, so to compile multiple files you need to do something like:

{% highlight bat %}
dir *.ts /b /s > ts-files.txt
tsc @ts-files.txt
{% endhighlight %}

This 2 steps will create a configuration file containing the arguments (the list of files in this case) that will be passed in to the TypeScript compiler.

**Source Map**

We all like a good debugging experience, since many browsers now support sourcemap, let's instruct TypeScript to generete them for us:

{% highlight bat %}
tsc app.ts --sourcemap
{% endhighlight %}

This command will generate .map (sourcemap) files.

**Join files**

{% highlight bat %}
tsc file1.ts file2.ts --out final.js
{% endhighlight %}

**ECMAScript target version**

By default the compiler will emit ES3 compatible JavaScript, to change it you need the '-t' switch (allowed values ES3, ES5, ES6).

{% highlight bat %}
tsc app.ts -t ES5
{% endhighlight %}

**Watch for changes**

Runing the compiler manually is quite tedious, we can ask the compiler to watch for file changes and recompile them on the fly:

{% highlight bat %}
tsc app.ts --watch
{% endhighlight %}

To use the watch feature case you will need to run the compiler you installed using npm and node, the version of the standard SDK will not provide watch support;
see my previous post on how to setup TypeScript to see how to find that version of the compiler

A typical command-line usage combines some or all of these switches:

{% highlight bat %}
tsc file1.ts file2.ts --sourcemap --watch
tsc file1.ts file2.ts --out combined.js --sourcemap --watch
{% endhighlight %}

**tsconfig.json**

TypeScript 1.5 also introduced support for a configuration file called [tsconfig.json](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json) that can be used to configure all the parameters the compiler support and to provide a list of files to compile; once again wildcards cannot be used in path definitions.

{% highlight json %}
{
    "compilerOptions": {
        "target": "ES5",
        "out": "./final.js",
        "sourceMap": true
    },
    "files" : [
        "./app.ts"		
    ]
}
{% endhighlight %}

Using a tsconfig.json file working with the command line compiler is pretty straightforward, the only real problem is the lack of wildcards support in defining the files to be compiled.
This problem can be easily solved using task runner like [Grunt](http://gruntjs.com/) or [Gulp](http://gulpjs.com/), and will be the subject of another post.

However keep in mind that, even if using those solution, if you’re using wildcards in your paths, any new files created since running the tsc command won’t get compiled, you need to stop the watcher and start again.

_cya next_