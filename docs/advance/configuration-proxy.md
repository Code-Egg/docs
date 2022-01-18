---
layout: default
title: Proxy
parent: Advance
permalink: /docs/advance/proxy
nav_order: 5
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}

</details>

## Setting up OpenLiteSpeed as a reverse proxy

OpenLiteSpeed can be set up as a transparent reverse proxy to any backend web server or application server that supports HTTP. OpenLiteSpeed proxies to other servers by setting them as external applications.
Once you have set up your web server external application, you will have to set which traffic OpenLiteSpeed should send to that external application. This can be done in a number of ways: via rewrite rules, contexts, or script handlers.

### Create a Web Server External Application 

You do do it either Server level or Virtual Host level. 
To set up external application at server level, you add a Web Server in the **WebAdmin Console**: Navigate to **Server Configuration > External App > Add > Type**

The most important setting is Address. Set your backend server up to listen on this address and port. For simplicity, we've set our copy of Apache to listen on port 8080.
    - **Name** = `apache`
    - **Address** = `localhost:8080`
    - **Max Connections** = `100`
    - **Initial Request Timeout** = `60`
    - **Retry Timeout** = `0`
    - **Response Buffering** = `No`

Now you have OpenLiteSpeed ready to proxy to your backend server. next, we need to setup rewrite rules, contexts, or script handlers so 
OpenLiteSpeed knows what traffic to send to this external application. 

**Note:** If you are using IP-based virtual hosting, you will need to set up a different web server external application for each vhost, as each web server external application only reroutes traffic to a single IP address.

### Method 1: Proxy with Context
{: .no_toc}
Proxying with contexts allows you to add other context functionality on top of your proxying.
Using a proxy context to designate which traffic to proxy has the advantage of allowing you to easily set aside a location on your site to be proxied. More than that, though, it allows you to add the other functionality of a context — adding headers, requiring authorization, only authorizing certain users, etc. — on top of the proxying.
Contexts only exist within virtual hosts, so you will need to have a vhost for the content you are proxying. For this guide we will be using the default virtual host, `Example`.

#### Create Context

Add a Proxy context from the **WebAdmin Console**: Navigate to **Virtual Host Configuration > [Example] > Add > Type > Proxy**
With the following settings, we will be proxying all the locations on this site to the web server internal app.
  - URI = `/`
  - Web Server = `[Server Level]: apache`

### Method 2: Proxy with Script Handlers

Setting up a script handler mapped to your web server external application allows you to easily proxy certain types of requests (determined by the suffix used in the request). This can be a simple way to set up OpenLiteSpeed to handle most content, but send requests for certain special content (Java scripts or scripts that require Apache, to name two examples) to a different backend. In this guide we will demonstrate how to proxy html files to an Apache backend.

#### Create a Script Handler
{: .no_toc}

Add a Script Handler from the **WebAdmin Console**: Navigate to **Virtual Host Configuration > [Example] > Script Handler > Add**
  - Suffixes = `html`
  - Handler Type = `Web Server`
  - Handler Name = `apache`

### Method 3: Proxying with Rewrite Rules
{: .d-inline-block }

name-based virtual hosting
{: .label .label-green }

One of the most versatile ways to designate which traffic to send is through rewrite rules. We think rewrite rules are the most convenient way to proxy all traffic for a specific virtual host. Rewrite rules can do far more than just this, though. By editing rewrite rules as specified in Apache's [mod\_rewrite documentation](http://httpd.apache.org/docs/current/mod/mod_rewrite.html), one can direct the server to proxy traffic based on suffix or URI or many other variables.

#### Create a vhost for the Site to Be Proxied

##### WebAdmin console \> Virtual Hosts \> Add

    Virtual Host Name: proxy-vhost
    Virtual Host Root: $SERVER_ROOT/proxy/
    Config File:  $SERVER_ROOT/conf/vhosts/$VH_NAME/vhconf.conf
    Max Keep-Alive Requests: 1000
    Follow Symbolic Link: No
    Enable Scripts/ExtApps: No
    Restrained: Yes

Save and graceful restart to apply changes.

#### Add a Rewrite Rule in the vhost to Send Traffic to the Proxy External Application

WebAdmin console \> Virtual Hosts \> proxy-vhost \> Rewrite
Set Enable Rewrite to YES

##### For IP-Based Virtual Hosting

If you're using IP-based virtual hosting on your backend (each domain has its own IP), then you can use the REWRITE RULES as follows:

    REWRITERULE ^(.*)$ HTTP://apache:8080/$1 [P]

**Note:** "apache" is the name of a proxy (web server) external application you have created.
**Note:** If you are using IP-based virtual hosting, you will need to set up a different web server external application for each vhost, as each web server external application only reroutes traffic to a single IP address.
**Note:** Add "https://" in front if the external web server uses HTTPS. Port is optional if the external web server uses the standard ports 80 or 443.

##### For Name-Based Virtual Hosting

If you're using name-based virtual hosting (many domains using one IP), you need to add another variable to your rewrite rule to signal which backend virtual host you intend the traffic to go to: (This ability is unique to LiteSpeed and our web server as an external application setup.)
Assume we have set up both port 80(apachehttp) and 443(apachehttps) External Applications.

    RewriteCond %{HTTPS} !=on
    REWRITERULE ^(.*)$ HTTP://apachehttp/$1 [P,L,E=PROXY-HOST:WWW.EXAMPLE1.COM]
    RewriteRule ^(.*)$ HTTPS://apachehttps/$1 [P,L,E=PROXY-HOST:WWW.EXAMPLE1.COM]

**Note:** "apachehttp" and "apachehttps" is the name of a proxy (web server) external application you have created. "www.example1.com" is the domain name of the backend vhost to be proxied.
**Note:** Add "https://" in front if the external web server uses HTTPS. Port is optional if the external web server uses the standard ports 80 or 443.

#### Map the Proxy vhost to Your Listener(s)

WebAdmin console \> Listeners \> your listeners \> Virtual Host Mappings \> Add

    Virtual Host: proxy-vhost
    Domain: proxy-vhost.domain.com

#### <span id=".28Optional.29_Enable_per-client_throttling_for_the_vhost_.28to_provide_HTTP-level_anti-DDoS_protection.29" class="mw-headline">(Optional) Enable Per-Client Throttling for the vhost (to provide HTTP-level anti-DDoS protection)</span>

WebAdmin console \> Virtual Hosts \> proxy-vhost \> Basic

    Per Client Throttle
    Static Requests/second: 50
    Dynamic Requests/sec: 5
    Outbound Bandwidth (bytes/sec): 100K
    Inbound Bandwidth (bytes/sec): 20K

 

#### (Optional) Log Real Visitor IP in backend

Enable **Use Client IP in Header **per this [document](https://openlitespeed.org/kb/show-real-visitor-ip-instead-of-cloudflare-ips/)
Now OpenLiteSpeed will forward server environment variable **REMOTE\_ADDR** and **X-Forwarded-For** headers.
Example code for an Apache virtual host configuration to log the real visitor IPs:

``` line-numbers
SetEnvIf REMOTE_ADDR "(.+)" CLIENTIP=$1
SetEnvIf X-Forwarded-For "^([0-9.]+)" CLIENTIP=$1
LogFormat "%{CLIENTIP}e %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" CLIENTIP
CustomLog  /path/to/access.log CLIENTIP
```

 
**NOTE**: For more advanced situations, please see [Advanced Reverse Proxy](https://openlitespeed.org/kb/advanced-reverse-proxy/).
