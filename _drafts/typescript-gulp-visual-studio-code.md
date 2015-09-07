---
layout: post
title: TypeScript, Gulp and Visual Studio Code
comments: true
tags: [typescript, gulp, vscode]
---

edit the .settings/tasks.json to tell VSCode which gulp tasks it has access to:

// Available variables which can be used inside of strings.
// ${workspaceRoot}: the root folder of the team
// ${file}: the current opened file
// ${fileBasename}: the current opened file's basename
// ${fileDirname}: the current opened file's dirname
// ${fileExtname}: the current opened file's extension
// ${cwd}: the current working directory of the spawned process

// Use Gulp as the task runner
{
    "version": "0.1.0",
    "command": "gulp",
    "isShellCommand": true,
    "args": [
        "--no-color"
    ],
    "tasks": [
        {
            "taskName": "build-ts",
            //"args": [],
            "isBuildCommand": true,
            //"problemMatcher": "$msCompile",
			"showOutput": "always"
        },
		{
            "taskName": "watch",
            //"args": [],
            "isBuildCommand": false,
            //"problemMatcher": "$msCompile",
			"showOutput": "always"
        },
		{
            "taskName": "watch-2",
            //"args": [],
            "isBuildCommand": false,
            //"problemMatcher": "$msCompile",
			"showOutput": "always"
        }
    ]
}

Ctrl+Shift+p -> Run Task

Ctrl+Shift+b -> Build (isBuildCommand = true)