---
layout: post
title: AngularJs 1.x and TypeScript best practices - bindToController
comments: true
disqus_identifier: 964B2565-7487-4E85-BF54-3172D41EBCEC
tags: [angular, typescript, bindToController]
---

One of my best practices when creating new Angular components using TypeScript is to use classes when defining Service(s) and Controller(s) and to use the 'controllerAs' syntax for the bindings.

You can take a look at the material from my session at the Italian AngularConf2015 in the previous posts to have a better idea of what I mean. Some code samples are also available to better illustrate the matter. 

There's however a problem when it comes to the 'controllerAs' syntax and directives that define an isolated scope and use properties on that scope  to bind values from the outside world.
When using controllerAs Angular 'hides' the $scope object and binds directly to the properties of the controller, the $scope is however still there and you can define properties on it like this:

{% highlight js %}
class controller {
	public test: string // a property on the controller
}

function directive(): ng.IDirective {
	return <ng.IDirective>{
		controller: "controller",
		controllerAs: "vm",
		// an isolated scope!
		scope: {
			test: "=" // a property of the scope
		}
	}
}
{% endhighlight %}

This is the usual way you have to define a two-way binding on the 'test' property with the outside boundary of the directive. 

You can run the code and using a tool like [Batarang](https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk) or [ng-inpector](http://ng-inspector.org/) you can easily verify that you'll end up having 2 distinct copies of the 'test' property: one on the controller and one on the $scope. Both being out of sync.

One way to overcome this issue is to use the [bindToController](https://docs.angularjs.org/api/ng/service/$compile) property:

{% highlight js %}
function directive(): ng.IDirective {
	return <ng.IDirective>{
		controller: "controller",
		controllerAs: "vm",
		// an isolated scope!
		scope: {
			test: "=" // a property of the scope
		},
		bindToController: true
	}
}
{% endhighlight %}

With this declaration the properties on the controller class will be kept in sync with the related properties on the underlying $scope object.

Good practice: **if you use a TypeScript class to define the controller of a directive, don't forget to set 'bindToController: true' when using the 'controllerAs' syntax.**

Angular 1.4 supports an extended syntax for the 'bindToController' property that allow us to specify the binding information using an object literal:

{% highlight js %}
function directive(): ng.IDirective {
	return <ng.IDirective>{
		controller: "controller",
		controllerAs: "vm",
		scope: {}, // an isolated scope!
		bindToController: {
			test: "=" // a property of the scope
		}
	}
}
{% endhighlight %}

This is important because bindToController has been extended to be used with every kind of directive that introduce a new scope (even the non isolated ones).

My Best Practice: **when defining a directive that uses an isolate scope, always specify the bindable properties using the extensive 'bindToController' syntax.**

This article was based on:
- AngularJs version 1.4+
- TypeScript version 1.5+

_cya next_