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

* Before You Begin
* Pointing your domain name to Linode name servers
* Adding blog subdomain to your domain name
* Installing Hugo
* Creating a Hugo blog article
* Installing Nginx
* Configuring Nginx as a reverse proxy server
* Learning more about Hugo

## Before You Begin

1.  Familiarize yourself with our [Getting Started](/docs/getting-started) guide and complete the steps for setting your Linode's hostname and timezone. The guide explains how to sign-up, deploy a Ubuntu 16.04 image and connect to your Linode server using secure shell protocol (SSH). For the purpose of installing Hugo and configuring Nginx, it is not necessary to do the "Setting the Hostname" steps.

2.  This guide will use `sudo` wherever possible. Complete the sections of our [Securing Your Server](/docs/security/securing-your-server) to create a standard user account, harden SSH access and remove unnecessary network services.

3.  Update your system:

        sudo apt-get update && sudo apt-get upgrade


## Pointing your Domain Name to Linode Name Servers

There are two aspects to pointing your registered domain name to your Linode server. Linode DNS Manager needs to be configured with your domain name and the web hosting site from which your domain name was purchased/registered needs to be configured with the address of Linode's name servers. Configuration steps for the Linode side can be found at [DNS Manager Overview](/docs/networking/dns/dns-manager-overview/). A general overiew of [DNS Records](/docs/networking/dns/dns-records-an-introduction/) is also available.

### Set Linode Name Servers on Web Hosting

The typical scenario is that you purchased and registered a domain name through one of the popular shared web hosting provider. Now you need to have the domain name point to your Linode server. Navigate to the Control Panel on your shared web hosting.

<p align="center">
  <img src="/images/ControlPanel.jpg" alt="Control Panel" /> 
</p>


## Adding blog subdomain to your domain name
## Installing Hugo
## Creating a Hugo blog article
## Installing Nginx
## Configuring Nginx as a reverse proxy server

### Running Hugo server at the command-line

### Tips and Tricks

## Learning more about Hugo