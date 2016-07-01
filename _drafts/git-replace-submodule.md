---
layout: post
title: Git - Replace a Submodule
comments: true
disqus_identifier: 7937849D-7624-4866-9F73-7F782EAE0785
tags: [git]
---

Replacing a submodule is not a thing you do so often, it's especially needed if you create and maintain provate forks of projects or open sourced library and you want them to compile against all your forked repositories.

Once you have added a submodule with:

{% highlight bat %}
git submodule add --name NAME https://github.com/NEventStore/NEventStore.git
{% endhighlight %}

A new file named '.gitmodules' will be created in the project root folder;
this file will hold the reference to the submodule:

{% raw %}
[submodule "dependencies/NEventStore"]
	path = dependencies/NEventStore
	url = https://github.com/NEventStore/NEventStore.git
{% endraw %}

Replacing it is not that easy using the command line you can try to 'deinit' the module and add a new one with something like:

{% highlight bat %}
git submodule deinit NAME
git submodule add --name NAME https://github.com/NEventStore/NEventStore.git
{% endhighlight %}

But you will probably end up facing an error telling you that NAME was already in the index and to pickup a new name. 
It seems there's no easy way to say that you're not interested in a submodule anymore.

What I usually do to replace a Submodule is described in this quite long process:

- run 'git submodule deinit NAME'.
- edit the abovementioned 'gitmodules' file and replace/remove the module.
- move to the folder: '.git/modules' and the delete the cache of the old submodule.
- delete the folder containing the previous copy of the submodule.
- move to the root folder of the git repo and switch to a development branch.
- run: 'git submodule init' followed by 'git submodule update' to init and clone the submodule repository.
- move to the folder where your submodule is located and checkout the branch you want to use (you might need to fetch all once again).
- verify that everything compiles correctly and push the changes to repository.