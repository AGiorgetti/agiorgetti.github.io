---
layout: post
title: TypeScript - Interfaces
comments: true
tags: [typescript, interface]
---

An interface is a way to give a 'name' to things, it defines a code contract within our code, it's a way to specify the 'public' shape that the 'entities' should have.

Objects that implement an interface are guaranteed to have the members and the functione exposed; nothing is said about the internal implementation of the object itself.

*example of interface*

Interfaces are a key concept in TypeScript because of the way the type checking system work.
 
TypeScript type checking is based on the concept called *Structural Typing* or *Duck Typing*: when comparing two interfaces (or two types) what really matter is the shape the objects have, not the 'name' associated with the interface.

This leads to the conclusion that if two different interfaces expose the same properties, they 'match' and the compiler does not have anything to complain when you try to assign one object to the other, or if you use one object in place of the other (as argument of a function maybe).

*example of interfaces and assignments*

Interfaces can describe:

- Objects
- Functions
- Arrays / Dictionaries
- Hybrid Types ('things' that are both objects and functions)

Interfaces support:

- Inheritance

They do not support accessors (get / set): you need to convert the 'property' to a 'getProperty()' function if you want that readonly behavior.