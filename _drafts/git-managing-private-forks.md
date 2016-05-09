---
layout: post
title: Git - Managing private forks
comments: true
disqus_identifier: 7A2821F5-36AB-415E-B715-B19F7F297EE3
tags: [git]
---

This is what I do when I need to keep and maintain a fork of a project:

- fork the project (this can be done in Github).

- clone the project locally:

{% highlight bat %}
git clone https://github.com/AGiorgetti/NEventStore.git
{% endhighlight %}

- Add an upstream remote to the original repository

{% highlight bat %}
git remote add upstream https://github.com/NEventStore/NEventStore.git
{% endhighlight %}

- I keep the standard branch names: 'master' and 'develop' in sync with the original project, so I start by creating my two new 'develop' and 'master' branches (out of the original ones):

{% highlight bat %}
git checkout master
git branch grd/master
git chekcout develop
git branch grd/develop
{% endhighlight %}

Everything is good to go and now I can start working on my private branches.

Whenever I need to resync the my private fork with the remote repository what I usually do is this:

- Bring the two official branches in sync with the remote ones (I except not errors or conflicts at this point because I have not made any change to the files in these branches):

{% highlight bat %}
git fetch --all
git checkout master
git merge upstream/master
git checkout develop
git merge upstream/develop
{% endhighlight %}

- Now I want to merge the changes made to the original project to my development branches. I do that using 'rebase' (because I want to keep a linear history if possible):

{% highlight bat %}
git checkout grd/master
git rebase master
...
git checkout grd/develop
git rebase develop
{% endhighlight %}

*Creating a Release*

- Releases can be made only from the master branch
- Each release should be marked by a tag 'grd.vX.X.X'

{% highlight bat %}
git tag grd.vX.X.X
git push --tags
{% endhighlight %}