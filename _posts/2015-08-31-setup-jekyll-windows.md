---
layout: post
title: Setup Jekyll on Windows
comments: true
tags: [jekyll, setup]
---

This post will open a series of articles in which I'll describe how I migrated my blog to Jekyll; I know there are tons of posts like this one on the field, but I need this one as a reminder to myself of how I did it.

For complete and extensible informations on what Jekyll is and how it operates head to the [official Jekyll website](http://jekyllrb.com/).

Let's start by having a local copy of Jekill running on my development machine:
- to install what is needed to run Jekyll you can follow this very good step-by-step guide by Julian Thilo: [Run Jekyll on Windows](http://jekyll-windows.juthilo.com/)

If you want to be able to watch for files modification and regenerate the website everytime you change something, pay special attention to the last section of the guide [step 4 - watch](http://jekyll-windows.juthilo.com/4-wdm-gem/);
I needed to manually install a compatible version of the 'listen' gem in order to have everything working on my machine.

At this point you have everything in place to create your blog, you have two ways of doing that:

- use Jekyll command line tools to have itself generate a basic blog template:

{% highlight c %}
jekyll new blogname
{% endhighlight %}

This command will create a new folder contaning all the files.

- clone an existing repository which contains the template and the skin you'll like to use for you blog: I liked the [Notepad](https://github.com/hmfaysal/Notepad) theme, so I started cloning that, but if you look around you can find many themes to use as a basic building block.

Once you have everything in place you are ready to start serving you blog locally with:

{% highlight c %}
jekyll serve --drafts -w
{% endhighlight %}

This command will enable 'draft' support, build the blog, serve it and start watching for local modifications.

_cya next_