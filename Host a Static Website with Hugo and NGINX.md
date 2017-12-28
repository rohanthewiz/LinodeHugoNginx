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
* Pointing your domain to Linode
* Adding blog subdomain
* Installing Hugo
* Creating a blog article
* Installing Nginx
* Configuring Nginx
* Learning more about Hugo

## Before You Begin

1.  Familiarize yourself with our [Getting Started](/docs/getting-started) guide and complete the steps for setting your Linode's hostname and timezone. The guide explains how to sign-up, deploy a Ubuntu 16.04 image and connect to your Linode server using secure shell protocol (SSH). For the purpose of installing Hugo and configuring Nginx, it is not necessary to do the "Setting the Hostname" steps.

2.  This guide will use `sudo` wherever possible. Complete the sections of our [Securing Your Server](/docs/security/securing-your-server) to create a standard user account, harden SSH access and remove unnecessary network services.

3.  Update your system:

        sudo apt-get update && sudo apt-get upgrade


## Pointing your Domain to Linode

There are two aspects to pointing your registered domain name to your Linode server. Linode DNS Manager needs to be configured with your domain name and the web hosting site from which your domain name was purchased/registered needs to be configured with the address of Linode's name servers. Configuration steps for the Linode side can be found at [DNS Manager Overview](/docs/networking/dns/dns-manager-overview/). A general overiew of [DNS Records](/docs/networking/dns/dns-records-an-introduction/) is also available.

### Set Linode Name Servers on Web Hosting

The typical scenario is that you purchased and registered a domain name through one of the popular shared web hosting provider. Now you need to have the domain name point to your Linode server. Navigate to the Control Panel on your shared web hosting.

<p align="center">
  <img src="/images/ControlPanel.jpg" alt="Control Panel" /> 
</p>

Click on the link that lists all the domains or the domain you have registered. In this case I'll click on DomainCentral located in the Domain Tools section.

<p align="center">
  <img src="/images/MyDomains.jpg" alt="Domain Listing" /> 
</p>

From the list of domains select the one you want to point to your Linode server. In the case where you have only one domain name registered, select that one. On selecting that domain name, an overview of various actions that can be taken on the domain name will be presented.

<p align="center">
  <img src="/images/Overview.jpg" alt="Overview of possible actions" /> 
</p>

In this case, I'll select Points to URL icon so as to enter the IP address of my Linode server.

<p align="center">
  <img src="/images/Pointers.jpg" alt="Point URL to Linode" /> 
</p>

Now that the URL is pointing to the IP address of your server on Linode, the Linode name servers need to be added.

<p align="center">
  <img src="/images/Nameservers.jpg" alt="Name servers" /> 
</p>

The following name server entries should be made:
* ns1.linode.com
* ns2.linode.com
* ns3.linode.com
* ns4.linode.com
* ns5.linode.com

It is best of follow the instructions provided for your specific scenario. The scenario discussed above applies to previously purchasing a domain name through one of the common shared web hosting providers such as Bluehost, Godaddy, or Fatcow.


## Adding blog subdomain

One to the things you may want to do is add a subdomain to your domain and have this url point to your blog articles. For example if your domain is http://example.com adding a blog subdomain creates http://blog.example.com. Log into your Linode server account and select the DNS Manager tab.

<p align="center">
  <img src="/images/AAAA.jpg" alt="AAAA records" /> 
</p>

In the section for A/AAAA Records select the link Add a new A record.  For Hostname add: http://blog.example.com . For IP Address enter the IP address of your Linode server. 

<p align="center">
  <img src="/images/EditAAAA.jpg" alt="Editing AAAA records" /> 
</p>

{{< note >}}
Your IP address can be found by selecting the Linodes tab. Replace example.com with your registered domain name. Save the Changes to the A/AAAA Record. Subdomain blog should now appear in the list of A/AAAA Records.
{{< /note>}}

## Installing Hugo

Open a terminal window and log in to your Linode server at the command line with SSH. A typical command line log in should look as follows:

    ssh username@IPAddress

Further clarification on logging in with SSH can be found in the [Getting started](/docs/getting-started) guide in the section titled *Log In for the First Time*. Create a directory named hugo and change into this directory. 

    cd ~
    mkdir hugo
    cd hugo

Use command wget to get the latest Hugo release files. Latest releases are located at [Hugo releases](https://github.com/gohugoio/hugo/releases).

    wget https://github.com/gohugoio/hugo/releases/download/v0.31.1/hugo_0.31.1_Linux-64bit.deb

To get the link to the release you wish to obtain right click on the release link and select Copy Link. Paste in the terminal window.

<p align="center">
  <img src="/images/CopyLink.jpg" alt="Copy release links" /> 
</p>

Install the package using the Terminal console with the following command:

    sudo dpk -i hugo *.deb

Installation of Hugo can be confirmed by checking the version of Hugo that was installed:

    hugo version

Another way to test the installation is with the Hugo help command:

    hugo help

Results should be similar to:

```bash
hugo is the main command, used to build your Hugo site.

Hugo is a Fast and Flexible Static Site Generator
built with love by spf13 and friends in Go.

Complete documentation is available at http://gohugo.io/.

Usage:
  hugo [flags]
  hugo [command]

Available Commands:
  benchmark   Benchmark Hugo by building a site a number of times.
  config      Print the site configuration
  convert     Convert your content to different formats
  env         Print Hugo version and environment info
  gen         A collection of several useful generators.
  help        Help about any command
  import      Import your site from others.
  list        Listing out various types of content
  new         Create new content for your site
  server      A high performance webserver
  undraft     Undraft resets the content's draft status
  version     Print the version number of Hugo

Flags:
  -b, --baseURL string             hostname (and path) to the root, e.g. http://spf13.com/
  -D, --buildDrafts                include content marked as draft
  -E, --buildExpired               include expired content
  -F, --buildFuture                include content with publishdate in the future
      --cacheDir string            filesystem path to cache directory. Defaults: $TMPDIR/hugo_cache/
      --canonifyURLs               if true, all relative URLs will be canonicalized using baseURL
      --cleanDestinationDir        remove files from destination not found in static directories
      --config string              config file (default is path/config.yaml|json|toml)
  -c, --contentDir string          filesystem path to content directory
      --debug                      debug output
```





## Creating a blog article
## Installing Nginx
## Configuring Nginx as a reverse proxy server

### Running Hugo server at the command-line

### Tips and Tricks

## Learning more about Hugo