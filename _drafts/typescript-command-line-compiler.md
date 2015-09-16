---
layout: post
title: TypeScript command-line Compiler
comments: true
tags: [typescript]
---

Let's have another introductory post on TypeScript. This time it's the command-line Compiler turn.

If you use a fully featured IDE with TypeScript support - like Visual Studio 2013+ - you will probably do not have to worry about the command-line compiler.
It's nonetheless usefull to have an overview on some of its basic functionalities; 
it can come in handy when you do not have an IDE available or just to automate some tasks with a task runner.

**Compile a single file:**

{% highlight bat %}
tsc app.ts
{% endhighlight %}

Generates app.js.

**Compile multiple files**

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

**Joining files**

{% highlight bat %}
tsc file1.ts file2.ts --out final.js
{% endhighlight %}

It will concatenate all the generated JavaScript on a single file called final.js.

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

In a Windows environment, to use the watch feature you will need to run the compiler you installed using npm and node; the version of the standard SDK will not provide watch support.
See my previous post [Setup TypeScript](http://www.primordialcode.com/blog/post/setup-typescript/) on how to setup TypeScript to see how to find the version of the compiler you need.

A typical command-line usage combines some or all of these switches:

{% highlight bat %}
tsc file1.ts file2.ts --sourcemap --watch
tsc file1.ts file2.ts --out combined.js --sourcemap --watch
{% endhighlight %}

**tsconfig.json**

TypeScript 1.5 also introduced support for a configuration file called [tsconfig.json](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json) that can be used to configure all the parameters the compiler supports and to provide a list of files to compile; once again wildcards cannot be used in path definitions.

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

Working with the command line compiler while using a tsconfig.json file is pretty straightforward, the only real problem is the lack of wildcards support to define the paths of the files to be compiled.
This problem can be easily solved using task runner like [Grunt](http://gruntjs.com/) or [Gulp](http://gulpjs.com/), and will be the subject of another post.

However keep in mind that, even if using those solution, if you’re using wildcards in your paths, any new files created since running the 'tsc' command won't get compiled, you'll need to stop the watcher and start it again.

_cya next_