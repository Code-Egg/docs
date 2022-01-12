---
layout: default
title: VirtualHost
parent: Configuration
nav_order: 2
---

# Setting up Name-based Virtual Hosting on OpenLiteSpeed

## Name-Based Virtual Hosting on OpenLiteSpeed

Name-based virtual hosting assigns virtual hosts based on domain names, not IP addresses. With name-based virtual hosting, you can host more than one website (virtual host) on each IP address.
Use the following guidelines to set up name-based virtual hosting:

## <span id="Set_up_DNS_properly" class="mw-headline">Setup DNS Properly</span>

Forward the domain names of your web sites to the IP address used by your web server. This is commonly done by adding an "A" name entry to the DNS zone file for the website. This is not part of your OpenLiteSpeed configurations.

## Setup the Virtual Hosts in the Webadmin Console

### Create a Virtual Host for Each Website

First, make the virtual host's directories. I will name my virtual host "Example2" (because I've used Example and Example3 for other guides). In the command line, I go to my LSWS directory and make the following directories:

    cd /usr/local/lsws

    mkdir Example2

    mkdir Example2/{conf,html,logs}

I then make conf owned by lsadm:lsadm (the WebAdmin console user) so that only the WebAdmin console will be able to manipulate configurations. (You should not allow other users permission to manipulate your configuration.):

    chown lsadm:lsadm Example2/conf

Then I go to the WebAdmin console \> Virtual Hosts \> Add to add the virtual hosts to OpenLiteSpeed:
![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-vh-add-new-1024x371.png)
You have to enter the virtual host's name, the virtual host root file, and the virtual host configuration file. You also need to choose whether to enable scripts on this site and whether users can access content outside of this virtual host root from the site (Restrained).
![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-vh-settings-new-1024x464.png)
I don't have a virtual host configuration file yet, because I'm starting from scratch, so I tell OpenLiteSpeed to make one for me:
![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-vh-settings2-new-1024x457.png)
Then I save, go back into the virtual host's configurations, and specify my document root:
![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-vh-docroot-new-1024x388.png)

### Create and Assign Listeners

Go to the WebAdmin console \>  Listeners.
You can have one listener to listen on all local IP addresses, or you can create multiple listeners with each listener only listening to a specific IP address. Many users will find it simpler to have one listener that is then mapped to different domains, but having multiple listeners can be useful if, for example, you wish to set aside certain server processors for certain sites (see the [listener binding section](http://www.litespeedtech.com/docs/webserver/config/listeners/#listenerBinding) of LSWS's documentation) or conduct special functions on separate ports.
I don't need anything special, so I just go to the Default listener (that listens to all IPs on port 80):
![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-vh-listener-new-1024x272.png)
And add a new mapping:
![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-vh-add-listener-mapping-new-1024x450.png)
And input the domain for my virtual host: (In the Domains setting, "your.domain" will match to both "www.your.domain" and "your.domain". The leading "www." in a domain name is ignored.)
![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-vh-add-listener-mapping2-new-1024x361.png)

### <span id="Graceful_restart" class="mw-headline">Graceful Restart</span>

![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-restart-new-1024x438.png)
And we're done\! :)
![](https://openlitespeed.org/wp-content/uploads/2018/06/namebased-vh-works-new-1024x355.png)
**Note:** OpenLiteSpeed supports Server Name Indication (SNI), allowing users to set SSL certificates at the virtual host level. This means that virtual hosts (websites) with different SSL certificates can operate on the the same IP address and port number. Different listeners (and IP-based hosting) are not necessary for secure sites to have unique certificates.
