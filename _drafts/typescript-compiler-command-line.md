---
layout: post
title: Using the TypeScript command-line Compiler
comments: true
tags: [typescript]
---

If you use a fully featured IDE with TypeScript support - like Visual Studio - you will probably do not have to worry about the command-line compiler;
it's nonetheless usefull to have a grasp on some of its basic functionalities, it can come in handy when you do not have those IDE available or just to automate some tasks with a task runner.

tsc app.ts

generates app.js

tsc file1.ts file2.ts, file 3.ts
tsc *.ts

compile multiple files, wildcards are supported.

Source Map

--sourcemap

Join files

tsc *.ts --out app.js

-t (type ES3, ES5)

Watch for changes

tsc *.ts --watch

If you’re using a wildcard like this, any new files created since running the tsc command won’t get compiled, you need to stop the watcher and start again.

a typical usage combines some or all of these switches:

tsc *.ts --sourcemap --watch
tsc *.ts --out combined.js --sourcemap --watch

