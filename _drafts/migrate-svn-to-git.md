---
layout: post
title: Migrate SVN to GIT
comments: true
disqus_identifier: 35A733F0-DF47-4615-94C4-0F4AC1CECA85
tags: [git, svn]
---

I know there are plenty of articles on the subject, but I needed to keep track how I did it. To migrate my current SVN repositories to Git I started to look around for some documentation and ended up with these ideas in mind:

- Clone the SVN repository to a local Git Repository while filtering the data and converting the users.
- Create a Remote (bare) Git repository that will be your new 'master' repository.
- Choose which branches of the local repository push to the new remote Git repository.

These are the steps I followed:

**Step 1 - Install**

Install [Git](https://git-scm.com/) if you do not have it already.

**Step 2 - Map the users**

Create an 'authors.txt' file that will be used to map your SVN users to Git users; it's just a plain text file that will look like this:

{% highlight bat %}
SVN_Username = Git_Username <user@mail.com>
...
{% endhighlight %}

**Step 3 - SVN clone**

Clone the SVN repository with [git svn](https://git-scm.com/docs/git-svn):

{% highlight bat %}
```git svn clone --stdlayout --authors-file=authors.txt --ignore-paths="packages/|packages$" --prefix="svn/" http://your.svn/repo LocalGitFolder```
{% endhighlight %}

The parameters used are:

- stdLayout: use this if you are using the standard SVN folder layout.
- authors-file=authors.txt: you need this file to map your users.
- ignore-paths="packages/|packages$": regex that specify which paths you'll like to ignore. 
- prefix="svn/": setup a prefix that will be used to map your svn remotes (otherwise 'origin/' will be used).

**Step 4 - CheckOut branches**

List all the available branches and choose the ones you want to push to your new remote Git repository:

{% highlight bat %}
git branch -a
{% endhighlight %}

You will see a brand new and shiny 'master' branch (a local branch) and some remote branches (coloured in red) named something like: remotes/svn/trunk, remotes/svn/branches, etc...

By default the master branch will point to your SVN trunk branch; the others (the remotes/svn/something) are all your other branches and tags you had in your SVN repository. 

To choose which of them you want to push to the remote Git repository you just have to perform a checkout on each and every one of them:

{% highlight bat %}
git checkout svn/branchName (you get a detached branch)
git checkout -b svn/branchName
{% endhighlight %}

If you want to import all of them, you can issue this commands to the Git bash:

{% highlight bat %}
for remote in `git branch -r`; do git checkout $remote; git checkout -b $remote; done
{% endhighlight %}

If you now list all your branches again, your will see some more local branches (in white).

You can push to a remote Git repository only your local branches, not the remote svn ones.

**Step 4 [alternative] - there's also another way**

There's also another way to deal with the remote _svn branches_ and _svn tags_ and how to 'change' them so they will appear as proper Git Branches and Git Tags.

Take a look at the page [Git and Other Systems - Migrating to Git](https://git-scm.com/book/it/v2/Git-and-Other-Systems-Migrating-to-Git) of the Git Documentation.  

**Step 5 - Push**

Create your new remote Git repository, in my case I decided to use [Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx),  attach it as a remote to the local Git repository (I used 'vsts' as a name for the remote reference) and push to it: 

{% highlight bat %}
git remote add vsts https://myAccount.visualstudio.com/DefaultCollection/myColl/_git/myRepo
git push vsts --mirror -u 
{% endhighlight %}

Done!

**Step 6 - bonus: keep it in sync**

If you want to keep the things in sync for a while and keep using your old SVN repository while experimenting and getting familiar with Git, you can issue the following commands to the Git bash:

{% highlight bat %}
git svn fetch
git rebase remotes/svn/trunk svn/trunk
git rebase remotes/svn/trunk master
git push vsts
{% endhighlight %}

With these commands you will:

- Update all your local 'remotes/svn' branch.
- 'Merge' (ok it's a rebase, I know, [check the docs](https://git-scm.com/docs/git-rebase)) all the new commits to your current 'master' and the imported 'svn/trunk' branches.
- Push the changes to the remote repository.

_cya next_