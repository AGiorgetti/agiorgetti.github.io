---
layout: post
title: Jekyll: be careful with the syntax highlight, it can cause build fails
comments: true
tags: [jekyll, publish]
---

Jekyll on GitHub uses liquid tags and [Pygments](http://pygments.org/) as its default syntax highlighter.
It generally works great but sometimes can cause troubles to the Jekyll build process, especially if you mistype the language it should render.

This is the syntax you should use:

<pre><code>
{% highlight html %}
{{ "{{ “{% highlight languageTag " }}}}%}
	
{{ "{{ “{% endhighlight " }}}}%}
{% endhighlight %}
</code></pre>

The problem is: **if you misstype the 'languageTag' changes are that Jekyll is not able to build the site anymore** and the returning message from GitHub isn't of mu help,
in the emails that were sent to me i could read:

"The page build failed with the following error:
Page build failed. For more information, see https://help.github.com/articles/troubleshooting-github-pages-build-failures.
If you have any questions you can contact us by replying to this email."

A little bit more details would have been appreciated.

It took me some time of investigation, trial and error to spot the problem because I wasn't using pygments in my local Jekyll installation.

To test any modification I don't use 'Pygments', but 'Rouge' as my syntax highlighter (I don't like the idea to install pyton just to have this one running).
It turns out that if you misstype the language tag Rouge doesn't complain. 

For a complete list of the languages supported by Pygments - and their short name tag - you can look at this [page](http://pygments.org/docs/lexers/).

_cya next_