---
layout: post
title: Using Git
comments: true
disqus_identifier: GUID
tags: [git]
---

Git Internal Workflow

We usually should work on a common 'develop' branch, so push and pulls should be done on that branch only.
Follow the [Atalassian Centralized Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/centralized-workflow).

__Develop Branch__

1- Clone the Remote Repository:

{% highlight bat %}
git clone https://sidsrl.visualstudio.com/DefaultCollection/_git/GitTest
{% endhighlight %}

2- Always work on 'develop':

{% highlight bat %}
git checkout develop
git pull --rebase
{% endhighlight %}

using --rebase we as git to place all our commits ON TOP of the work done by other poeple. 

3- Commit your work __locally__:

{% highlight bat %}
git status
git add .
git commit -m "comment your commit"
{% endhighlight %}

If you had to resolve any conflict (due to the rebase):

{% highlight bat %}
git add .
git rebase --continue
{% endhighlight %}

4- Push to the remote repository:

{% highlight bat %}
git push develop
{% endhighlight %}

__Private Branch__

1- Create a new feature branch

{% highlight bat %}
git checkout develop
git pull
git branch feature/branch1
git checkout feature/branch1
{% endhighlight %}

2- Commit your work __locally__:

{% highlight bat %}
git status
git add .
git commit -m "comment your commit"
{% endhighlight %}

3 (option1) - Merge your work back to develop:

Commit your work __locally__:

{% highlight bat %}
git checkout develop
git pull
git merge feature/branch1
{% endhighlight %}

3a- no conflicts!

{% highlight bat %}
git push
git branch feature/branch1 -d
{% endhighlight %}

3b- oh no! conflicts!!

Resolve the configcts using your tools and:

{% highlight bat %}
git add .
git commit -m "merge feature/branch1 on develop"
git push
{% endhighlight %}

This will create a new commit with all the files modified in the branch (if you have done more than 1 local commit, these will be translated to a single new commit containing all the modified files at once).

__FALSE__
_If you do not PUSH the new branch to the remote repository (origin) they will have no knowledge at all of what you have done in your branch._
__FALSE__
_If you delete the local branch, all the history if your commit will be lost (you might consider to push it to the server to keep the history safe)._

All the commits in your 'local' branches will be sent to the server anyway, thus you can end up having a 'mess':

[put an image here]

3 (option2) - Rebase your work back to develop:

To place your feature branch commits ON TOP of develop commits do:

{% highlight bat %}
git checkout feature/branch2
git rebase develop
{% endhighlight %}

{% highlight bat %}
git checkout develop
git rebase feature/branch2
git branch feature/branch2 -d
git push
{% endhighlight %}

To Place your feature branch commit BELOW new develop branch commits do:

{% highlight bat %}
git checkout develop
git pull --rebase
git rebase feature/branch2
{% endhighlight %}

resolve your conflicts and:

{% highlight bat %}
git rebase --continue
git branch feature/branch2 -d
git push
{% endhighlight %}

__Release__

1- Merge Develop onto Master
2- Create a Version Tag
3- Use this version to build a release.