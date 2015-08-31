---
layout: post
title: Setup TypeScript
comments: true
tags: [typescript, setup]
---

**Prerequisites**

Node and NPM have to be installed, so navigate to [NodeJs official website](https://nodejs.org/) and grab the installer best suited for your system.

To test if it's installed and if it work you can open your command prompt(or bash or whatever you use) and type these two commands:

{% highlight c %}
node -v
npm -v
{% endhighlight %}

both will show the version number of the installed Node and NPM.

**Installing TypeScript**

To install TypeScript open a command pront and type:
{% highlight c %}
npm install -g typescript
{% endhighlight %}

The -g option will install the package globally, so that the ocmpiler can be used in any project.
To test if it works just type:
{% highlight c %}
tsc -v
tsc -h
{% endhighlight %}

To print out the compiler version number and an help of all the available options.

**Fixing the Issue with multiple versions installed**

It's always a good idea to check if you have the latest version available, if the displayed version number it's not what you expect it to be - especially after having updated all your global packages - 
chances are you have multiple copies of the package installed on your machine, and the default path is pointing you to an outdated one.
Fix the issue on a Windows machine is not that hard, just look and the paths defined in the environment system variables and point the to the newly installed version of typescript.

In my case I had to open the 'Control Panel', select 'System' and then 'Advance System Settings'; looking and the System enviroment variables, under the PATH option I could read:
...C:\Program Files (x86)\Microsoft SDKs\TypeScript\1.0\...
Which was pointing to an outdated version of the compiler, find where the newly installed version is on your system (usually inside C:\Program Files (x86)\Microsoft SDKs\TypeScript) and fix the path. 

As a hint you can:

- install a propert SDK from Microsoft, and look under C:\Program Files (x86)\Microsoft SDKs\TypeScript
- manually update the npm package (npm install -g typescript) and take note of the paths that will be printed out by the installer:
{% highlight c %}
C:\Temp\typescript>npm install -g typescript
C:\Users\Username\AppData\Roaming\npm\tsserver -> C:\Users\Username\AppData\Roaming\npm\node_modules\typescript\bin\tsserver
C:\Users\Username\AppData\Roaming\npm\tsc -> C:\Users\Username\AppData\Roaming\npm\node_modules\typescript\bin\tsc
typescript@1.5.3 C:\Users\Username\AppData\Roaming\npm\node_modules\typescript
{% endhighlight %}

**Keep TypeScript Updated**

To update nodejs global packages, you can use:
{% highlight c %}
npm install -g typescript
{% endhighlight %}

To find out which packages need to be updated, you can use:
{% highlight c %}
npm outdated -g --depth=0
{% endhighlight %}

To update all global packages, you can use:
{% highlight c %}
npm update -g
{% endhighlight %}

**Grab a good editor, possibly with TypeScript support**

There are many editors out there, some are listed on the [TypeScript](http://www.typescriptlang.org/) Website.

On a Windows OS I'd like to recommend:

- [Visual Studio](https://www.visualstudio.com/) - the community edition is free 
- [Visual Studio Code](https://code.visualstudio.com/) 

On other operating systems you can check Visual Studio Code as well, it's currently available for Linux and Mac OSX too.

_cya next_