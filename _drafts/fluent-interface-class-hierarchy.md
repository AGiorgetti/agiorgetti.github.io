---
layout: post
title: Fluent Interface and Class Hierarchy in C#
comments: true
disqus_identifier: 54F15EEF-C85D-47B0-B089-D5530838FE99
tags: [C#, fluent interface]
---

Do you like to have [fluent interfaces](https://en.wikipedia.org/wiki/Fluent_interface) in your code because they make it more readable?

Do you have complex a class hierarchy and you'd like to have a fluent interface without the need of always returning the base class type? and possibly avoid all the cast(s) in the methods' chain?

Here's the standard way of approaching Fluent Interfaces with inherited classes in C#:

{% highlight csharp %}
public class Calculator
{
    public Calculator Add()
    {
        // do the job
        return this;
    }
}

public class ScientificCalculator : Calculator
{
    public ScientificCalculator Sin()
    {
        // do the job
        return this;
    }
}

public class FluentInterfacesAndClassHierarchies
{
    public void FluentInterface_And_ClassHierarchy()
    {
        var scientificCalculator = new ScientificCalculator();

        scientificCalculator
            .Add();
        //  .Sin(); // you cannot call this method here! the return value of Add() is a normal Calculator!
        // you will need to cast, and that will break the chain!
    }
}
{% endhighlight %}

As you can see it just doesn't work out of the box! You can overcome the issue using _extension methods_, here's how:

{% highlight csharp %}
public class Calculator
{
    public Calculator Add()
    { ... same as above ... }

    // we change the implementation to internal and we add an extension method.
    internal Calculator internalSubtract()
    {
        // do the job
        return this; // actually there's no real need to return the instance of the object here.
    }
}

/// <summary>
/// Extension Methods for Fluent Interfaces to the rescue
/// </summary>
public static class CalculatorFluentInterface
{
    public static T Subtract<T>(this T calculator) where T : Calculator
    {
        calculator.internalSubtract();
        return calculator;
    }
}

public class ScientificCalculator : Calculator
{ ... same as above ... }

public class FluentInterfacesAndClassHierarchies
{
    public void FluentInterface_And_ClassHierarchy()
    {
        var scientificCalculator = new ScientificCalculator();

        scientificCalculator
            .Subtract() // Type inference guarantees that the return value of the extension method is the original type.
            .Sin(); 
    }
}    
{% endhighlight %}

Surely defining the api in this way is not that pretty, but that will be hidden inside your library and the users will be happy!

_cya next_