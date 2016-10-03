---
layout: post
title: AngularJs 1 to Angular 2 migration - setup
comments: true
disqus_identifier: GUID
tags: [angularjs,ng,angular]
---

#Setup the environment.

- Install [nodejs](https://nodejs.org/en/)
- Let Visual Studio know that you have an updated version of node installed (instead of using the built in version): go to 'Tools -> Options -> Projects and Solutions -> External Web Tools" and add a new path there pointing to the newly installed node (usually in 'C:\Program Files\nodejs'), be sure that's the first in the list.
- keep [NPM](https://www.npmjs.com/) updated: follow the instruction in this [link](https://docs.npmjs.com/getting-started/installing-node) and run:

```npm install npm -g```

You will need to install NPM because it is the primary distribution source for Angular 2 packages.

#Add TypeScript to an existing Asp.NET MVC 5 project

Follow the instructions at this [link](https://github.com/Microsoft/TypeScript-Handbook/blob/master/pages/tutorials/ASP.NET%204.md)

#Add Angular 2 to you existing project (Hybrid Bootstrap)

You'll need to add Angular libraries and probably setup a build using a Task Runner and some utilities. It all depends on how you want to serve your application.

The root of the application must always be and AngularJs 1 component.

##Adding Angular 2 to an Asp.NET MVC 5 project

(and serving Angular 2 using the standard Asp.NET bundles ?)

Angular 2 brings in a totally modular approach with lazy loading capabilities based on the 'new' module loaders available (SystemJS, Webpack, etc...).

##NgModule

It's a good idea to read the official documentation on [NgModule](https://angular.io/docs/ts/latest/guide/ngmodule.html), this way you have a better idea of what a module is in Angular 2 and how it differs from the concept of a module in AngularJs 1.

Angular registers modules' providers with the application root injector.
Angular module instances, unlike components, do not have their own injectors so they can't have their own provider scopes.

Warning: lazy loaded modules create their own injector in the injector tree (see [NgModule FAQ](https://angular.io/docs/ts/latest/cookbook/ngmodule-faq.html#!#q-lazy-loaded-module-provider-visibility)).

Warning: when it comes to define feature modules and application wide singletons keep in mind this (form the docs): 

        Do not specify app-wide singleton providers in a shared module. A lazy loaded module that imports that shared module will make its own copy of the service.

To group up all the application wide singletons we should use Core Modules (it'a a normal module), these core moduler regigester the services in their 'providers' section. The root application module is the only one that should import Core Modules.

##Hynrid Bootstrap


