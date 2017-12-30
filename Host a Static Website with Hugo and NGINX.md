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
  - '[Reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy)'
---

Hugo is promoted as "...one of the most popular open-source static site generators." In this article we will discuss installing 
[Hugo](https://gohugo.io/) on Ubuntu 16.04, then discuss installing and configuring [Nginx](http://nginx.org/) as a reverse proxy. We will begin by pointing your registered domain name to Linode name servers. The ultimate goal is to enter a URL such as http://blog.example.com in your web browser to display your Hugo blog articles hosted on Linode.

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

{{&lt; note &gt;}}
Your IP address can be found by selecting the Linodes tab. Replace example.com with your registered domain name. Save the Changes to the A/AAAA Record. Subdomain blog should now appear in the list of A/AAAA Records.
{{&lt; /note &gt;}}

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

If you are getting started with creating blog articles with Hugo the best source of accurate and updated instructions is the Hugo site [Getting Started Quick Start Guide](https://gohugo.io/getting-started/quick-start/). Instructions begin at Step 2: *Create a New Site*.

## Installing Nginx

Now that you have a basic Hugo blog post created, the next issue is making it accessible to the public using your domain name. Hugo blog has a built-in server that runs, by default, at port 1313. This means that if Hugo was installed on your local machine, such as a desktop or laptop, it is accessible with http://localhost:1313/ . 

Since our Linode machine is remote, using http://example.com:1313 will not give us access to the Hugo blog without further configurations to our Ubuntu installation. Providing direct port access can increase vulnerabilities to web-based attacks. Using Nginx protects your Linode server against common vulnerabilities and can be configured to make several server instances publicly accessible on port 80 by adding sub-domains to your domain name. 

In our case we added blog sub-domain to create URL http://blog.example.com .

<p align="center">
  <img src="/images/Nginxrevproxy.jpg" alt="Nginx as Reverse proxy" /> 
</p>

Let's [install Nginx](http://nginx.org/en/linux_packages.html) and check that it is running.

    sudo apt-get update
    sudo apt-get install nginx
    systemctl status nginx

If the Nginx server is running as expected the following output should be displayed:

```bash
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2017-12-14 10:48:55 CST; 1 weeks 6 days ago
  Process: 2332 ExecStop=/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.pid (code=exited, status=0/SUCCESS)
  Process: 25525 ExecReload=/usr/sbin/nginx -g daemon on; master_process on; -s reload (code=exited, status=0/SUCCESS)
  Process: 2360 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
  Process: 2339 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
 Main PID: 2371 (nginx)
    Tasks: 2
   Memory: 2.7M
      CPU: 1.014s
   CGroup: /system.slice/nginx.service
           ├─2371 nginx: master process /usr/sbin/nginx -g daemon on; master_process on
           └─2374 nginx: worker process  
```


## Configuring Nginx

There are a number of commands that will become necessary for starting, stopping, reloading, and checking the status of the nginx server as changes are made to the nginx server configuration file /etc/nginx/sites-available/default.

    // Check server status
    systemctl status nginx

    // Reload Nginx without stopping
    systemctl reload nginx

    // Stopping Nginx server
    systemctl stop nginx

Modify Nginx server configuration with the following command:

    cd ~
    sudo vim /etc/nginx/sites-available/default

The most challenging aspect of making your Hugo blog publicly available with Nginx is getting the configuration correct. The arrangement below shows the configuration for two server applications. The first allows http://example.com/ to access a server application running on port 8081 and the second http://blog.example.com/ will access our Hugo blog running on port 1313. 

```bash
# Default server configuration
#
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name www.example.com example.com;

        location / {
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $remote_addr;
                proxy_set_header Host $host;
                proxy_pass http://127.0.0.1:8081;
        }
}

server {
        listen 80;

        root /var/www/html;

        server_name blog.example.com;

        location / {

                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $remote_addr;
                proxy_set_header Host $host;
                proxy_pass http://127.0.0.1:1313;
        }
}
```

### Running Hugo server at the command-line

An equally challenging issue to configuring Nginx, is issuing the correct command to get the Hugo blog running correctly. Consider the following command with runs blog articles in draft stage using [Material Design](https://themes.gohugo.io/material-design/) theme.

    hugo server -t material-design -D

Output from the Terminal console will be similar to:

```bash
Started building sites ...

Built site for language en:
1 of 1 draft rendered
0 future content
0 expired content
1 regular pages created
8 other pages created
0 non-page files copied
2 paginator pages created
0 tags created
0 categories created
total in 8 ms
Watching for changes in /home/your-username/hugo/{data,content,layouts,themes,static}
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop

```

Note that the web server is available at http://locahost:1313/. If you attempt to enter your domain name http://blog.example.com/ in a web browser, content from your blog will appear. However, it will appear without the CSS formatting. Viewing the web browser page source will show that links to page CSS files include http://localhost:1313/. Clearly what we need is something similar to http://example.com/css/styles.css. We can achieve the correct CSS links by adding the baseUrl and appendPort setting in the command to run the Hugo server.

    hugo server -t material-design -D --baseUrl="http://blog.example.com" --appendPort=false &

This command outputs the listing below to the Terminal console.

```bash
Started building sites ...

Built site for language en:
1 of 1 draft rendered
0 future content
0 expired content
1 regular pages created
8 other pages created
0 non-page files copied
2 paginator pages created
0 tags created
0 categories created
total in 8 ms
Watching for changes in /home/your-username/hugo/{data,content,layouts,themes,static}
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://blog.example.com/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

Hugo is now available at http://blog.example.com/ and not http://localhost:1313 as before in the Terminal console. 

{{&lt; note &gt;}}
Notice the use of the & at the end of the command to run Hugo. This keeps the Hugo server running and allows us to logout and close the console terminal. Start typing then hit the Return key to return to the command prompt without stopping the Hugo server.
{{&lt; /note &gt;}}

We should now be able to access our blog article in a web browser with http://blog.example.com/.

### Tips and Tricks

There are times when shutting down the server with Ctrl + C doesn't do the trick. This will be apparent the next time we attempt to start-up. The console will display something to the effect, "Web Server is available at http://localhost:12345. Port 12345, in this example, is clearly not correct. It is now necessary to shutdown the server running on port 1313. Shutting down the process on port 1313 can be done with:

    sudo kill $(sudo lsof -t -i:1313)

## Learning more about Hugo

Now that your basic blog site is accessible to the world on via your domain name, you will want to increase the depth of your knowledge about the mechanics of Hugo. [Hugo docs](https://gohugo.io/documentation/) is a great place to start on this quest. [Giraffe Academy](https://www.youtube.com/watch?v=qtIqKaDlqXo) also has a series of great Hugo video tutorials.
