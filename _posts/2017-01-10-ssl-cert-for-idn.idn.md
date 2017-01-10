---
layout: post
title: SSL Certificate for idn.idn domain
tags: ssl domain
---

I got a SSL certificate for an idn.idn domain recently.

Here it is: [https://你我.中国/](https://你我.中国/)

It might not sound like a big deal, but it actually is. It had not been possible for close to 10 years since the existense of idn.idn.

IDN stands for "internationalized domain name", basically it allow domain names to have non-ascii characters in them.  `idn.idn` means the tld, or top-level domain, such as .com, .net, .cn .uk, .in etc. i.e. the last part of domain name, also has non-ascii characters in it. They can be Chinese, Russian, Arabic, etc.

For a long time, it's not possible to get SSL certificate for IDN, then it's available, but only for idn.tld, not idn.idn. If your tld is non-ascii, no SSL for you.

Last year, letsencrypt.org gave domain owners the ability to get free SSL certificate, I tried to find out if it support idn.idn, it didnt.

I went to github intending to file an issue with letsencrypt and found out just 2 days earlier, someone already created a [pull request](https://github.com/letsencrypt/boulder/pull/2278).

After 2 months, the issue is fixed.  I was able to get a certificate for my idn.idn.  Thank you letsencrypt and open-source community.
