---
layout: post
title: Git - tips and tricks
comments: true
disqus_identifier: GUID
tags: [git]
---

Git Installation

Be sure to specify a default behavior for:

- CheckIn / CheckOut EOL style

Be sure to correctly configure the Git username for the local repository
So your commits are correctly associated with your user (if you use a Credential Manger).

        git config --edit (--system, --global, --local)

and set the options:

[user]
    name = xxx
    email = yyy@yyy.com
