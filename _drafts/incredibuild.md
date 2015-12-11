---
layout: post
title: IncrediBuild
comments: true
disqus_identifier: GUID
tags: [TAGS]
---

Visual Studio 2015 Update 1 come with a new _free_ bonus: You can download and install a new version of IncrediBuild.

If you install it through Visual Studio menu you'll get a version of IncrediBuild specifically designed for the environment that will allow you to keep using 8 core for parallel builds even when the trial license expires (normally it will be limited to use 4 core).

To install it use the "File \ New \ Project..." menu, then look for a link called "Build Accelerators", inside it you'll find the template to install IncrediBuild.

Once you open the template, the installation process starts:

1.png

2.png

Grab a free (for life) licence

Filling in the form will result in an EMail contiaing the lice file you'll need to use in order to enable the software

3.png

It states free for life, the licenses details states it's a 30 days trial (probably for the full features) and a 1 year licence for all the rest.



Once installed you are ready to use it.

Now you can choose go to with 2 different scenarios:
- use multiple helpers (already configured) machines to help you during the build process.
- go for a single machine installation and fine tune the settings for it.

Let's start with this second option, which is easier to start with if you are using IncrediBuild for the first time.

Open the IncrediBuild Agent Settings.

Go to the 'Initiator \ General' tab and:

- check 'Enable Standalone mode'.

4.png

Open the IncrediBuild Agent settings and check the 'Visual Studio Builds \ Advanced' tab and:

- check Predictive Execution - enanche throughtput using out-of-order tasks spawing
- uncheck PDB File Allocation - Limiti concurrent PDB

5.png

after this you are ready to go:

IncrediBuild - Build Solution 

You will get reports like:

6.png

you can now start using this insight look to hunt for compilation bottlenecks and possible refactorings to do in your code, as an example: start giving a look at projects that cannot be built in parallel early in the compilation chain, you can possibly refactor or relocate your code in order to improve the overrall build time. 

After playing a bit with the tool I was able cut 30 seconds from total compilation time lowering it to 1m 43s starting from 2m 10s, not that bad!

_cya next_