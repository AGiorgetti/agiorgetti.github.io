---
layout: post
title: A first look at IncrediBuild
comments: true
disqus_identifier: 6BBE13FE-A5C9-4553-8502-CBF821EEF7CD
tags: [IncrediBuild]
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

Let's start with this second option, which is easier to start with if you are using IncrediBuild for the first time (like me).

Open the IncrediBuild Agent Settings.

Go to the 'Initiator \ General' tab and:

- check 'Enable Standalone mode'.

4.png

On the IncrediBuild Agent settings and check the 'Visual Studio Builds \ Advanced' tab and:

- check Predictive Execution - enanche throughtput using out-of-order tasks spawing
- uncheck PDB File Allocation - Limiti concurrent PDB

5.png

After this you are ready to go, click on the top menu: IncrediBuild - Build Solution 

You will get a report like this:

6.png

You can now start using this insight look (it's always useful to have a graphical view of what is happening) to hunt for compilation bottlenecks and possible refactorings to do in your code, as an example: start giving a look at projects that cannot be built in parallel early in the compilation chain, you can possibly refactor or relocate your code in order to improve the overrall build time.

The rule of thumb is always to keep the dependecies among projects as low as possible.

After playing a bit with the tool I was able cut the total compilation by 10-15%, not that bad!

A deeper look at the report shows and initial 'preparing build task' period which is quite long (around 10s or more), I assume that this is the setup phase of incredibuild which tries to reach its compilation 'helpers' or creates a plan on how to parallelize builds. However since I setup for 'Standalone mode' I expected this step to be shorter.

Once I refactored my code I swtiched back to the usual Visual Studio compilation pipeline (which does some parallelized compilation itself) and I found out it to be faster, mostrly due to not having to deal with IncrediBuild initial setup task.

After using it for a couple of hours I have the strong feeling that on a single machine and with a reasonable low number of projects in your solution, you can use its graphical report to tweak your compilation at start, then switch back to the usual compilation pipeline.

I'll test it in a multi machine environment, in this scenario I expect it to have a bigger impact on the overral compilation experience (especially in case of build automation). 

_cya next_