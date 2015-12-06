---
layout: post
title: Using Letsencrypt
---

[Let's Encrypt](https://letsencrypt.org/) entered public beta 2 days ago. It also happened that I have a SSL certificate that just expired a few days ago.  I took this opportunity to try out LetsEncrypt.

Let's Encrypt basically use ACME protocol to validate you own(have control of) the domain, then issue you SSL certificate.

I used [simp_le](https://github.com/kuba/simp_le/) instead of official LetsEncrypt client since it's simpler to use and it's written by one of official client developers.

I will use example.com as an example for the domain name.

The client must be able to write files to the directory that can be publicly accessed from the address: http://example.com/.well-known/acme-challenge/

For static site, there's no problem as the client just writes files to 'document root'/.well-known/acme-challenge.  But for dynamic site like ones I have, my 'document root' is not fixed.

I have one site that use Nginx as reverse proxy, it pass all web requests to a backend server except those that request '/static/'.  In this case for simp_le to write files that can be accessed from '/.well-known/acme-challenge/', I have to either add an exception to Nginx or handle requests to '/.well-known/acme-challenge' from backend server.

To add an exception to handle '/.well-known/acme-challenge', add this before the 'location /' line in nginx server config:

    location /.well-known/ {
        root   /var/www/example.com/web/;
    }

Then use '/var/www/example.com/web/'as path during simp_le client invocation.

    $simp_le -d example.com:/var/www/example.com/web -f key.pem -f cert.pem -f fullchain.pem

For backend server handling, it depends on the type of server but basically just add a handler for URL: '/.well-known/acme-challenge/*' that return static files. I use Tornado as an example:

In urls section. add a handler:

    (r'/.well-known/acme-challenge/(.*)', tornado.web.StaticFileHandler, {'path': '/var/www/sites/example_com/server/.well-known/acme-challenge/'}),

Then use '/var/www/sites/example\_com/server' as path during simp_le client invocation.

    $simp_le -d example.com:/var/www/sites/example_com/server -f key.pem -f cert.pem -f fullchain.pem

If there's no error, you should get 3 files: key.pem, cert.pem, and fullchain.pem.  Put them in a certain location, for example: '/var/www/example.com/certs', then configure Nginx to use them:

    ssl on;
    ssl_certificate /var/www/example.com/certs/cert.pem;
    ssl_certificate_key /var/www/example.com/certs/key.pem;
    ssl_stapling on;
    ssl_stapling_verify on;
    ssl_trusted_certificate /var/www/example.com/certs/fullchain.pem;

I use Haproxy in front of Nginx, for Haproxy, do a `cat cert.pem fullchain.pem. key.pem > example.com.crt`, then put example.com.crt in the Haproxy's certificate directory.

I got a few sites that use letsencrypt ssl certificates now. Since this is so easy and cost no money, I will use letscrypt for all my sites (after the current ssl certificates expire).
