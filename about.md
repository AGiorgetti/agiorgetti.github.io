---
layout: page
permalink: /about2/index.html
title: Alessandro Giorgetti
---
<figure>
  <img src="{{ site.url }}/images/me.png" alt="Alessandro Giorgetti">
  <figcaption>Alessandro Giorgetti</figcaption>
</figure>

{% assign total_words = 0 %}
{% assign total_readtime = 0 %}
{% assign featuredcount = 0 %}
{% assign statuscount = 0 %}

{% for post in site.posts %}
    {% assign post_words = post.content | strip_html | number_of_words %}
    {% assign readtime = post_words | append: '.0' | divided_by:200 %}
    {% assign total_words = total_words | plus: post_words %}
    {% assign total_readtime = total_readtime | plus: readtime %}
    {% if post.featured %}
    {% assign featuredcount = featuredcount | plus: 1 %}
    {% endif %}
{% endfor %}

Giorgetti Alessandro has a Degree in Engineering, he likes to call himself a ‘Software Artisan’ and he’s co-founder of SID s.r.l. a company which mainly builds products and custom solutions for the Medical HealthCare System.

He's one of the founders of the .Net user group DotNetMarche and he’s an active speaker in many of the workshops the group organizes.

He's a Microsoft MCP and MCTS specialized in desktop and web application development using .Net technologies.

He likes sharing his experiences and experiments about WPF, Silverlight, WP7, ASP.Net, Linq, NHibernate and many more technologies through his blog.

He got involved in some Open Source projects, but actually his main focus is on Dexter Blog Engine.

Hobbies: .NET programming, Women, Heavy Metal, Online MMRPGs and Role Playing.

A quick note for the developers that play World of Warcraft too: if you lurk on Sunstrider (EU realm) you can find me there as Davion (one of the GMs of the guild Revenant, go check the Revenant website :D).
