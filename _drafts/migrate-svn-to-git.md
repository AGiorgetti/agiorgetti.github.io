---
layout: post
title: Migrate SVN to GIT
comments: true
disqus_identifier: GUID
tags: [git, svn]
---

To migrate my current SVN repositories to Git I started to look around for some documentation and ended up with this idea in mind:

- Clone the SVN repository to a local Git Repository, filtering the data and converting the users.
- Create a Remote (non interactive) Git repository that will be your new 'master' repository.
- Choose which branch of the local repository push to the new remote Git repository.

I followed the following steps:

**Step 1 - Install**

Install [Git](https://git-scm.com/) if you do not have it already.

**Step 2 - Map the users**

Create an 'authors.txt' file that will be used to map your SVN users to Git users; it's a plain text file that will look like this:

{% highlight bat %}
SYSTEM = SYSTEM <system@email.com>
guardian = guardian <alessandro.giorgetti@live.com>
{% endhighlight %}

**Step 3 - SVN clone**

Clone the SVN repository with [git svn](https://git-scm.com/docs/git-svn):

{% highlight bat %}
git svn clone --stdlayout --authors-file=authors.txt --ignore-paths="packages" --prefix="svn/" http://your.svn/repo LocalGitFolder
{% endhighlight %}

The parameters used are:

- --stdLayout: use this if you are using the standard svn folder layout.
- --authors-file=authors.txt: you need this file to map your users.
- --ignore-paths="packages": specify which paths you'll like to ignore. 
- --prefix="svn/": setup a prefix that will be used to map your svn remotes (otherwise 'origin/' will be used).

**Step 4 - CheckOut branches**

List all the available branches and choose the ones you want to push to your new remote Git repository:

{% highlight bat %}
git branch -a
{% endhighlight %}

You will see a brand new and shiny 'master' branch (a local branch) and some remote branches (coloured in red) named something like: remotes/svn/trunk, etc...

By default the master branch will point to your SVN trunk branch; the others (the remotes/svn/something) are all your other branches and tags you had in your SVN repository. 

To choose which of them you want to push to the remote Git repository you just have to checkout them:

{% highlight bat %}
git checkout remotes/svn/branchName
{% endhighlight %}

If you want to import all of them you can issue this commands to the Git bash:

{% highlight bat %}
for remote in `git branch -r`; do git branch --track $remote; done
git svn fetch --all
{% endhighlight %}

If you now list all your branches again your will now see some more local branches (in white).

You can push to a remote Git repository only your local branches, not the remote svn branches.

**Step 5 - Push**

Create your new remote Git repository, in my case I decided to use one on [Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx),attach it as a remote to the local Git repository and push it: 

{% highlight bat %}
git remote add vsts https://myAccount.visualstudio.com/DefaultCollection/myColl/_git/myRepo
git push vsts --mirror -u 
{% endhighlight %}

Done!

**Step 6 - bonus: keep it in sync**

If you want to keep the things in sync for a while and keep using your old SVN repository while experimenting and getting familiar with Git, you can issue the following commands to the Git bash:

{% highlight bat %}
git svn fetch
git rebase remotes/svn/trunk svn/master
git rebase remotes/svn/trunk master
git push vsts
{% endhighlight %}

With these commands you will:

- Update all your local 'remotes/svn' branches.
- 'Merge' all the new commits to your current 'master' and the imported 'svn/trunk' branches.
- push the changes to the remote repository.