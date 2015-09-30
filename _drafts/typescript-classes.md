---
layout: post
title: TypeScript - classes
comments: true
tags: [typescript, class]
---

Classes are a new feature to JavaScript that was introduced with ES6, TypeScript borrows the idea and offer you the possibility to use classes (with the same syntax you'll use in ES6) and output ES5 code.

A class comes into play when you want to provide behaviors to an entity.

They have support for:

- accessors (get, set) [ES5+]
- modifiers: public, private, protected
- constructor
- inheritable
- static properties
- abstract (class & methods)
- interface implementation

Classes also define Types, they have two sides:

- instance side (the properties involved in structural type checking)
- static side (constructor and static properties, not involved in the type checking)

*eXample of class goes here*

Pay attention to

The body of a class always behave like 'strict mode' was set, this has an impact because of how JavaScript's 'this' works inside functions.

The 'this': most of the times it represents the instance of the class itself (like in C#).

*example of this*

why is that? that's because of the functions get called on an instance of the class:
when a function is invoked as method of an object, the this will be instance of the object the function was called on.

Anothe thing to consider: if, for some reason, you need to define a function expression inside a class' method, like this:

*example of this inside a function expression*

the this might not be the instance of the class, thats because of how this behaves when used in function expression called under 'strict mode'
In this specif case, this will bbe 'whatever it was when entering the execution context of the function': it can be undefined or what was set from the outside (apply() call).
We have a workaround for 'this':

*show the workaround*

TypeScript has a better way of handling this scenarion: the arrow function syntax!

*show the syntax* *show the compied synta*

to sums up:

The 'this' has a different meaning in function expression and when using the 'arrow syntax':
function() { … }: this act exactly as expected in strict mode (it can be undefined or whatever it was when entering the function execution context).
() => { … }: this always refers to the class instance.

Composition / Encapsulation patterns: don't mess up with the this! Always delegate the function call properly, that is: call the function on its original object rather than assigning the pointer to the function to another variable!

*show a composition sample*

once again will not work because of the 'this' and because we are playing with function pointers here!

delegate the call to the service the proper way!
