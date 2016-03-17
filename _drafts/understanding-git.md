---
layout: post
title: Understanding Git
comments: true
disqus_identifier: 94A4A439-A494-4DB8-BEC0-935C01F96BB1
tags: [git]
---

Some experiments to understand [Git](https://git-scm.com).

**todo: checkin-checkout styles**

Create a repository:

- create a folder
- open a git bash and navigate to that folder
- type:

{% highlight js %}
git init
{% endhighlight %}

It will create a repo with a 'master' branch, the master will contains only the validated code, from the master we will be able to create release branches!.

Now create a file and commit it

{% highlight js %}
git status
git add .
git commit -m "..."
{% endhighlight %}

Now create a working branch

{% highlight js %}
git branch feature/1
git checkout feature/1
{% endhighlight %}

Add a new file (and commit) to feature/1, while someone else keep working on master.
Let's have this sequence of actions:

- (previous) commit on master
- commit on feature/1
- commit on master
- commit on feature/1
- commit on master

If we look at the graph we will see something like:

[Image 01]

Time to 'merge' feature/1 on master (no conflicts):

{% highlight js %}
git checkout master
git merge feature/1
{% endhighlight %}

A new commit will be created with all the modified files, the old branch will be kept, the image:

[Image 02]

The work was correcly merged, if you want you can not delete the branch, but you will loose your changes history!
Keep in mind that you cannot delete a branch which is not fully merged!

Let's try rebase:

- take the work done in your branch and reapply it on top of the work done in master
- it will create new commits (and delete your old ones).

{% highlight js %}
git checkout feature/1
git rebase master
{% endhighlight %}

and not merge it back to the master

{% highlight js %}
git checkout master
git merge feature/1
{% endhighlight %}

You will get a new 'linear' history (not loosing any of your change):

[Image 03]
[Image 04]

If you have conflicts along the way it can easily become a mess at every step!

Guideline is:
- rebase local
- do not rebase things you have pushed to a remote repo!
- merge remote

https://git-scm.com/book/it/v2/Git-Branching-Rebasing


