---
layout: post
title: Using PositiveSSL on Haproxy
tags: ssl haproxy https
---

I wrote about using PositiveSSL with Nginx [before](http://blog.wensheng.org/2012/03/using-namecheap-ssl-with-nginx.html).

After 4 years, for Haproxy, the steps are pretty much the same, except the last one.

From the issuing email, donwload the certificate file, unzip it, we got two files:
example_com.crt and example_com.ca-bundle

For Haproxy, we need to concatenate these two file with the private key file, but the order of concatenation is different from that for Nginx:

```bash
    cat example_com.crt example_com.ca-bundle example.com.key.nopass > example.com.crt
```

Then put example.com.crt in to the directory that`s specified by Haproxy configuration file and restart Haproxy.

(Update 2015-05) I got a new Positive ssl cert, but this time I got different files. The concatenation step now looks like:

```bash
    cat example_com.crt COMODORSADomainValidationSecureServerCA.crt COMODORSAAddTrustCA.crt AddTrustExternalCARoot.crt example.com.key.nopass > example.com.crt
```
