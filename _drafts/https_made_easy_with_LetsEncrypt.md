---
layout: post
title: Https made easy with Let's Encrypt
comments: true
disqus_identifier: 8571E8E3-C978-4F30-ADDF-37752478ACCD
tags: [https, certificate, encrypt]
---

When it comes to security you always need to take some actions to protect your data and your customer.

Having a website that runs Https is a must in the enterprise world.

So here are some simple step you can follow to configure your website:

- obtain a certificate from a Certificate Authority
- configure your website.

To obtain a ceritifcate you can look in the internet for a CA and buy one from them, or you can look out for some service that offers you the service for free.

One of those CA is [Let's Encrypt](https://letsencrypt.org/), from their about page: 

    "Anyone who owns a domain name can use Let’s Encrypt to obtain a trusted certificate at zero cost."

They also have a very nice tool that makes the certificate installation and maintenance almost painless (given you have full control over the machine that hosts your websites), the problem is: it does not work on Windows and you'll need to do some things all by yourself.

There are however a couple of good open source projects that can make your life easier:

- [ACMESharp](https://github.com/ebekker/ACMESharp) - a project that provides powershell scripts to interact with Let's Encrypt services.
- [Certify for Windows](http://certify.webprofusion.com/) - an open source GUI for ACMESharp, [github project](https://github.com/webprofusion/Certify).

Note: on windows machine it does not work without a valid HostName, you need to test it manually.





