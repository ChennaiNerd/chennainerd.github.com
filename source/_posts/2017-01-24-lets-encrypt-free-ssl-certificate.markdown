---
layout: post
title: "Secure your sites with Lets Encrypt Free SSL Certificates"
date: 2017-01-24 10:26:07 +0530
comments: true
author: Fizer Khan
categories:
---

SSL certificates are vital to protect communications between the web server and client.
We used to buy SSL certificates from [Symantec](https://www.symantec.com/ssl-certificates/),
[Verisign](https://www.verisign.com/en_IN/), [GeoTrust](https://www.geotrust.com), [RapidSSL](https://www.rapidssl.com/).
The price for SSL certificate will go from low to high for Single domain certificate, Wildcard certificate,
and Extended validation certificates respectively. Small startup or Blog owner cannot afford SSL certificates.

But you can get free SSL certificates through [Let’s Encrypt](https://letsencrypt.org/) which is
is a free, automated, and open Certificate Authority. It provides an easy way to obtain and install trusted certificates for free.
Lets see how we can get SSL certificate.

### Install Let’s Encrypt

In this tutorial, we are using Ubuntu 14.01. First we have to install Let’s Encrypt client
tool [certbot-auto](https://github.com/certbot/certbot)

    wget https://dl.eff.org/certbot-auto
    chmod a+x certbot-auto


### Getting new certificates

To obtain a cert for `acme.com`, `www.acme.com` and `blog.acme.net`:

    ./certbot-auto certonly --standalone --email admin@acme.com -d acme.com -d www.acme.com -d blog.acme.net

If you’re running a local webserver for which you have the ability to modify the content being served,
and you’d prefer not to stop the webserver during the certificate issuance process, you can use the
[webroot plugin](https://certbot.eff.org/docs/using.html#webroot) to obtain a cert by including certonly and
`--webroot` on the command line.

    ./certbot certonly --webroot -w /var/www/acme/ -d www.acme.com -d acme.com -w /var/www/blog/ -d blog.acme.net -d another.blog.acme.net

*Where:*

**-d** = Domain name

**-w** = Directory in which SSL certificates to be copied


### Renew Certificates

Let's Encrypt certificates will expire in 90 days after creation or renewal. You have to renew the certificates automatically before they expire. You can arrange for automatic renewal by adding a `cron` or `systemd` job which runs the following:

    ./certbot-auto renew --quiet --no-self-upgrade


You can find more detailed information and options in the [full documentation.](https://certbot.eff.org/docs/intro.html)

