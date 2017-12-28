---
author:
  name: Linode Community
  email: docs@linode.com
description: Guide for hosting Hugo on Ubuntu 16.04 with Nginx as reverse proxy.
keywords: ["Hugo", "Ubuntu 16.04", "Nginx", "reverse proxy"]
license: '[CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0)'
published: 2017-12-28
modified: 2017-12-28
modified_by:
  name: Linode
title: Host a Static Website with Hugo and NGINX
contributor:
  name:Patrick Allison
  link: '[GitHub](https://github.com/jaminyah)'
external_resources:
  - '[Hugo Getting Started](https://gohugo.io/getting-started/)'
  - '[Nginx docs](http://nginx.org/en/docs/)'
  - '[Running Hugo in the background](https://discourse.gohugo.io/t/run-server-as-background/3252)'
  - '[Setting appendPort and baseUrl](https://github.com/gohugoio/hugo/issues/852)'
  - '[Hugo styling not working](https://discourse.gohugo.io/t/hugo-static-styling-not-working-whereas-hugo-server-works/2916)'
---

Hugo is promoted as "...one of the most popular open-source static site generators." In this article we will discuss installing 
'[Hugo](https://gohugo.io/)' on Ubuntu 16.04, then discuss installing and configuring '[Nginx](http://nginx.org/)' as a reverse proxy. We will begin by pointing your registered domain name to Linode name servers. The ultimate goal is to enter a URL such as http://blog.example.com in your web browser to display your Hugo blog articles hosted on Linode.

Our discussion is best handled in increments by performing the following steps:

* Getting started with Linode
* Pointing your domain name to Linode name servers
* Adding blog subdomain to your domain name
* Installing Hugo
* Creating a Hugo blog article
* Installing Nginx
* Configuring Nginx as a reverse proxy server
* Learning more about Hugo


