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

- I keep the standard branch names: 'master' and 'develop' in sync with the original project, so I start by creating my two new 'develop' and 'master' branches (out of the original ones).

If your project also have submodule you need to keep them in sync with the specific branch you are using, so, after any checkout you should also do "git submodule update".

{% highlight bat %}
git checkout master
(git submodule update)
git branch grd/master
git chekcout develop
(git submodule update)
git branch grd/develop
{% endhighlight %}

Everything is now good to go and now I can start working on my private branches.

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

_submodules_

If you have changed the submodules to point to you provate fork and the remote repository has changed / updated their submodule you might end up having a conflict here which is not that easy to solve.
You will need to check two files:

- .git/config 
- .gimodules

Make yourself sure they still point to you private fork (these might have been changed by the merge operation) and they will not show up the 'git status' list of changed files to check.
To chance the commit at which your submodules point you should navigate inside those sub repositories and checkout the specific commit you need.

*Creating a Release*

- Releases can be made only from the master branch
- Each release should be marked by a tag 'grd.vX.X.X'

{% highlight bat %}
git tag grd.vX.X.X
git push --tags
{% endhighlight %}