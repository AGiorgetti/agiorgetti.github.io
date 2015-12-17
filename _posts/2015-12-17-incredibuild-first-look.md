---
layout: post
title: A first look at IncrediBuild
comments: true
disqus_identifier: 6BBE13FE-A5C9-4553-8502-CBF821EEF7CD
tags: [IncrediBuild, Visual Studio]
---

Visual Studio 2015 Update 1 come with a new _free_ bonus: You can download and install a new version of [IncrediBuild](http://www.incredibuild.com).

If you install it through Visual Studio you'll get a version of IncrediBuild specifically designed for the environment that will allow you to keep using 8 cores for parallel builds even when the trial license expires (normally it will be limited to 4 cores).

To install it use the "File \ New \ Project..." menu, then look for a link called "Build Accelerators", inside it you'll find the template used to install IncrediBuild.

Once you 'open' the link, the installation process starts; you will presented with a few choises:

![IncrediBuild Setup 1]({{ site.url }}/userfiles/images/incredibuild-first-look/1.png)

Let's install it on our local machine selecting option 1: "Activate IncrediBuild locally".

After that you will be be offered the option to request a free license, do that if you don't already own one.

![IncrediBuild Setup 2]({{ site.url }}/userfiles/images/incredibuild-first-look/2.png)

Filling in the form will result in an e-mail containing the license file you'll need to use in order to enable the software

![IncrediBuild Setup 3]({{ site.url }}/userfiles/images/incredibuild-first-look/3.png)

The disclaimer states it's free for life; however the licenses details states it's a 30 days trial (for the full featured version) and a 1 year licence for all the rest. Once the trial period expires you can keep using the product with some limitations in single machine mode (take a look at the official [IncrediBuild](https://www.incredibuild.com/microsoft-incredibuild-partnership.html) website for more information).

Once installed you are ready to use it.

Now you can choose to go with two different scenarios:

- Use multiple (already configured) helpers machines to assist you during the build process.
- Go for a single machine installation and fine tune the settings for it.

Let's start with this second option, which is easier to start with if you are using IncrediBuild for the first time (like me).

Open the IncrediBuild Agent Settings.
Go to the 'Initiator \ General' tab and check 'Enable Standalone mode'.

![IncrediBuild Setup 4]({{ site.url }}/userfiles/images/incredibuild-first-look/4.png)

Now swtich to the 'Visual Studio Builds \ Advanced' tab and:

- Check "Predictive Execution - enanche throughtput using out-of-order tasks spawing".
- Uncheck "PDB File Allocation - Limit concurrent PDB".

![IncrediBuild Setup 5]({{ site.url }}/userfiles/images/incredibuild-first-look/5.png)

Good to go, click on the top menu: "IncrediBuild - Build Solution" to do the dirty job!

You will get a report like this:

![IncrediBuild Report]({{ site.url }}/userfiles/images/incredibuild-first-look/6.png)

You can now start using this insight look (it's always useful to have a graphical view of what is happening) to hunt for compilation bottlenecks and possible ways to refactor your code; as an example: start giving a look at projects that cannot be built in parallel early in the compilation chain, you can possibly refactor or relocate your code in order to improve the overrall build time.

The rule of thumb is always to keep the dependecies among projects as low as possible, limiting the chain lenght in order to favor parallelism.

After playing a bit with the tool I was able cut the total compilation time by 10-15%, not that bad!

A deeper look at the report shows an initial 'preparing build task' period which is quite long (around 10s or more); I assume that this is the setup phase of IncrediBuild which tries to reach its compilation 'helpers' or creates a plan on how to parallelize the building tasks. However since I setup for 'Standalone mode' I expected this step to be shorter.

Once I refactored my code I switched back to the usual Visual Studio compilation pipeline (which does some parallelized compilation itself) and I found out it to be faster, mostly due to not having to deal with IncrediBuild initial setup task.

After using it for a couple of hours I have the strong feeling that on a single machine - with a reasonably limited number of projects in your solution - you can use its graphical reports to tweak your compilation at start, then switch back to the usual compilation pipeline.

I'll test it in a multi machine environment, in this scenario I expect it to have a bigger impact on the overral compilation experience (especially in case of build automation). 

_cya next_