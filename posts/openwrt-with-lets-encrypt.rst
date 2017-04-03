.. title: OpenWRT with Let's Encrypt
.. slug: openwrt-with-lets-encrypt
.. date: 2016-11-01 19:00:05 UTC-05:00
.. tags: openwrt, nginx, lets encrypt, ssl, https, draft
.. category:
.. link: 
.. description: How to create a server with self revalidating certificate
.. type: text

To create that blog I decided to use my home router `TP-LINK WR1043ND v1
<http://www.tp-link.com/en/download/TL-WR1043ND_V1.html>`_ to host Nginx server
with SSL to be more secure - yeah, it's XXI century and we should use SSL
everywhere especially that there is a `Let's Encrypt
<https://letsencrypt.org/>`_ project.

Why Let's Encrypt was created:

    The objective of Let’s Encrypt and the ACME protocol is to make it possible
    to set up an HTTPS server and have it automatically obtain a
    browser-trusted certificate, without any human intervention.

First you need to install required packages:

.. code:: bash

   opkg update opkg install nginx

Default version of Nginx provided by `OpenWRT.com` repositories doesn't provide
support for SSL. This feature is disabled so I had to compile that package on
my own.
