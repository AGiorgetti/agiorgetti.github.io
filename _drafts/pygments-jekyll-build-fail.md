---
layout: post
title: Jekyll - Pygments, mistyping the syntax highlight language tag results in blog build fail
comments: true
disqus_identifier: 6EF4C8B4-9FAA-49EE-8F1B-0E9EF66A2C49
tags: [jekyll, pygments]
---

Jekyll on GitHub uses liquid tags and [Pygments](http://pygments.org/) as its default syntax highlighter.
It generally works great, but sometimes it can cause troubles to the Jekyll build process, especially if you mistype the language it should output.

This is the syntax you should use:

{% raw %}
{% highlight languageTag %}
... your code goes here ...
{% endhighlight %}
{% endraw %}

The problem is: **if you mistype the 'languageTag' chances are that Jekyll is not able to build the site anymore** and the returning message from GitHub isn't of much help;
in the emails sent to me I could read:

>"The page build failed with the following error:
>Page build failed. For more information, see https://help.github.com/articles/troubleshooting-github-pages-build-failures.
>If you have any questions you can contact us by replying to this email."

A little bit more details would have been appreciated.

It took me some time of investigation, trials and errors to spot the problem, because I wasn't using pygments in my local Jekyll installation.

To test any change I don't use 'Pygments', but 'Rouge' as my syntax highlighter (I don't like the idea to install Pyton just to have this one running).
It turns out that if you mistype the language tag, Rouge doesn't complain with an error. 

For a complete list of the languages supported by Pygments - and their short name tag - you can look at this [page](http://pygments.org/docs/lexers/).

_cya next_