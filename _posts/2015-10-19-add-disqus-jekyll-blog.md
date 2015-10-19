---
layout: post
title: How to add Disqus to your Jekyll Blog
comments: true
disqus_identifier: EE3032CD-BAF7-4C15-9FB5-795F634DBC2C
tags: [jekyll, disqus]
---

1. Setup a [Disqus](https://disqus.com) account.
2. Hit 'Settings' - 'add Disqus to Site'.
3. Fill in the required informations for the site you'll like to add Disqus to.
![Disqus settings]({{ site.url }}/UserFiles/images/JekyllDisquss01.PNG)
4. After that you are asked to choose the platform in which 'install' the service; for Jekyll just pick 'Universal Code'. This will send you to a page in which you'll see instructions on which scripts to add to your templates and how to configure them.

Look for the proper position on your blog template (in the Notepad theme I used the file is: _includes/disqus_comments.html) and check/update the code if needed.

Done.

It was pretty simple right ?!

Yes!... But by default Disqus configuration uses the page url as the key to identify the comments related to a specific blog post, 
if you are migrating your data from another platform or you want the freedom to migrate away from Jekyll with an easy way to keep using your comments on Disqus, 
a good idea can be the following tip: 

**tip: use disqus_identifier and disqus_url**

Looking at the Disqus [JavaScript configuration variables](https://help.disqus.com/customer/portal/articles/472098-javascript-configuration-variables) documentation, 
they strongly encourage the use of these two variables: **disqus_identifier** and **disqus_url**.

Assigning a specific Disqus identifier to any post will make the very things easy as Disqus is going to use that id to retrieve your comments instead of the page url.
We can change our Jekyll templates again to include this values for us, let's design a solution:

- we want to be able to specify the disqus_identifier in the metadata (YAML front matter) of the post or page.
- if an explicit identifier was specified it will be used, otherwise we fall back to the page url.
- disqus_url variable must be explictly set to the page absolute path.

Here's how the YAML metadata will look like:

{% highlight YAML %}
---
layout: post
title: How to add Disqus to your Jekyll Blog
comments: true
disqus_identifier: A7655498-AB9E-40BF-A0D5-E5C6DE6BBF28
tags: [jekyll, disqus]
---
{% endhighlight %}

The Disqus template will be changed adding these lines:

{% highlight JavaScript %}
var disqus_identifier = '{% if page.disqus_identifier %}{{ page.disqus_identifier}}{% else %}{{ site.url }}{{ page.url }}{% endif %}';
var disqus_url = '{{ site.url }}{{ page.url }}';
{% endhighlight %}

Specifying these identifiers explicitly (I like to use GUIDs) will guarantee that they will be always consistent, even if you add a trailing backslash or some query string parameters to the browser address 
(the browser will navigate to the correct post anyway; but to Disqus, the two addresses are different and your comments could not be loaded).

After doing these two more steps you have Disqus correctly configured to work with your Jekyll website.

_cya next_