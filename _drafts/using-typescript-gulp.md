---
layout: post
title: Using TypeScript with Gulp
comments: true
tags: [typescript, gulp]
---

Gulp is a task runner that works on top of Node.js. 
As a prerequisite you should have Node.js already installed on your system.

Once you have Node.js installed (it will also install NPM - node package manager) you can open a console prompt and install Gulp globally:

{% highlight bat %}
npm install gulp -g
{% endhighlight %}

Installing it globally is a requirement for some IDE and editors like Visual Studio 2013 and Visual Studio Code, other IDEs - like Visual Studio 2015 - can operate with private copies.

The next step is initialize npm for the new project, so head to the root folder and type:

{% highlight bat %}
npm init
{% endhighlight %}

You will be asked some question and as a result a package.json file will be created. This file is used to define all the packages that will be installed and used locally by node.js.

We can start adding some plugins and tasks that we'll use to build our typescript files, let's start simple: edit the package.json file to add some dependencies.
To correctly compile TypeScript files using gulp we need the [gulp-typescript](https://github.com/ivogabe/gulp-typescript) plugin.

{% highlight json %}
{
  "name": "tsgulp",
  "version": "1.0.0",
  "description": "",
  "main": "final.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "alessandro giorgetti",
  "license": "ISC",
  "devDependencies": {
    "gulp": "^3.9.0",
    "gulp-typescript": "^2.8.2"    
  }
}
{% endhighlight %}

Once you'we done editing the file, run the command to download and install a local copy on the packages:

{% highlight bat %}
npm install 
{% endhighlight %}

It's time to define the task that will be used to compile the TypeScript files.
In the root folder of the project a _gulpfile.js_ file, then add this code:

{% highlight js %}
var gulp = require("gulp");
var ts = require("gulp-typescript");

gulp.task("build-ts", function () {
  var tsResult = gulp.src("*.ts")
    .pipe(ts({
        noImplicitAny: true,
        out: "final.js"
      }));
  return tsResult.js.pipe(gulp.dest('.'));
});
{% endhighlight %}

That's all that is needed to do in order to compile all your TypeScript files. 
You can already see the advantages of using task runners: you can define a build pipeline made of different actions that will change and trasform the build artifacts according to your desire.
You can chose which files to include and exclude from the compilation using the filtering capabilities of node.js and gulp (even using wildcards this time, a feature not yet supported by the plain TypeScript compiler),
However keep in mind that if you’re using wildcards in your paths, any new file created since running the tsc command may not be compiled, verify the behavior on your system because you might need to stop the watcher and start it again.

Using the TypeScript compiler through Node.js and Gulp (or any other task runner) you can also take advantage of the advanced features of the compiler,
like watching for every change made to the files and recompile them on the fly, all you need to do is add some more tasks to the gulpfile.js:

{% highlight js %}
var gulp = require("gulp");
var ts = require("gulp-typescript");

// just build all the typescript files
gulp.task("build-ts", function () {
  var tsResult = gulp.src("*.ts")
    .pipe(ts({
        noImplicitAny: true,
        out: "final.js"
      }));
  return tsResult.js.pipe(gulp.dest('.'));
});

// watch the files for changes and rebuild everything
gulp.task("watch", function() {
  gulp.watch("*.ts", ['build-ts']);  
});
{% endhighlight %}

Starting from here you can expand your build pipeline adding support for sourcemaps, automatic file references and reordering and so on...

_cya next_


