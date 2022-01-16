---
layout: default
title: Configuration
permalink: /configuration
nav_order: 3
---

---
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}

</details>
---

## Virtual Host Setup

With name-based virtual hosting, you can host more than one website (virtual host) on each IP address.

### Setup DNS Properly

Forward the domain names of your web sites to the IP address used by your web server. This is commonly done by adding an "A" name entry to the DNS zone file for the website. This is not part of your OpenLiteSpeed configurations.

### Create a Virtual Host directories

1. Make the virtual host's directories. I will name my virtual host "Example2" as an example. In the command line, I go to my LSWS directory and make the following directories:
```bash
mkdir /usr/local/lsws/Example2
mkdir /usr/local/lsws/Example2/{conf,html,logs}
```
I then make conf owned by `lsadm:lsadm` (the WebAdmin console user) so that only the WebAdmin console will be able to manipulate configurations.
```bash
chown lsadm:lsadm /usr/local/lsws/Example2/conf
```
2. Add an virtual hosts (in the WebAdmin console > Virtual Hosts > Add)
3. Give the new virtual hosts an appropriate name, Root, Config file. 
  - Virtual Host Name = Example2
  - Virtual Host Root = $SERVER_ROOT/Example2
  - Config File = $SERVER_ROOT/conf/vhosts/Example2/vhost.conf
  - Enable Scripts/ExtApps = Yes 
  - Restrained = No
4. You might see "file /usr/local/lsws/conf/vhosts/Example2/vhost.conf does not exist. CLICK TO CREATE" alert due to we starting from scratch, click the CLICK TO CREATE button so OpenLiteSpeed will make one for me. 
5. Click save button, go back into the Example2 virtual host's configurations, and specify the document root under the General tab
  - Document Root = /usr/local/lsws/Example2/html
  - Index Files = index.html, index.php
6. I'd also recommend you to enable the Rewrite feature which is under the Rewrite tab
  - Enable Rewrite = Yes
  - Auto Load from .htaccess = yes


## Listeners setup
### Listener create 
Go to the WebAdmin console > Listeners. The default listener that listens to all IPs on port 8088. You might want to create another two listeners for both HTTP and HTTPS ports.  
1. Click the Add button to create a HTTP listener with following settings:
  - Listener Name = HTTP
  - IP Address = ANY IPv4
  - Port = 80
  - Secure = No
2. Click the Add button to create a HTTPS listener with following settings:
  - Listener Name = HTTPS
  - IP Address = ANY IPv4
  - Port = 443
  - Secure = Yes
3. Navigate  to the WebAdmin Console > Listeners > HTTPS (or whatever name you used) > SSL, and click the Edit button. Specify the private key and certificate path, then click the Save button.
  Let's Encrypt
  - Private Key File = /etc/letsencrypt/live/example.com/privkey.pem (Use your own key path)
  - Certificate File = /etc/letsencrypt/live/example.com/fullchain.pem (Use your own cert path)
  - Chained Certificate = Yes
  Temporary certificate(web admin) or [private self-signed certificate](https://stackoverflow.com/questions/10175812/how-to-create-a-self-signed-certificate-with-openssl)
  - Private Key File = /usr/local/lsws/conf/example.key (Use your own key path)
  - Certificate File = /usr/local/lsws/conf/example.crt (Use your own cert path)
  - Chained Certificate = Not Set

### Virtual Hosts mapping:
Go to the WebAdmin console > Listeners > HTTP, click the Add button from the Virtual Host Mappings area and set with following settings. 
  - Virtual Host = Example2
  - Domains = example2.com, www.example2.com
Go to the WebAdmin console > Listeners > HTTPS, click the Add button from the Virtual Host Mappings area and set with following settings. 
  - Virtual Host = Example2
  - Domains = example2.com, www.example2.com

## SSL setup

**Note:** OpenLiteSpeed supports Server Name Indication (SNI), allowing users to set SSL certificates at the virtual host level. This means that virtual hosts (websites) with different SSL certificates can operate on the the same IP address and port number. Different listeners (and IP-based hosting) are not necessary for secure sites to have unique certificates.


## Cache Setup
Cache Module is Installed and Enabled by default, so we don't need to change any settings for it. 

You still need to enable caching for your web apps, and this can be done by [installing the corresponding LSCache plugin](https://docs.litespeedtech.com/lscache/#available-plugins), or using [rewrite rules](https://docs.litespeedtech.com/lscache/noplugin/) in .htaccess if no plugin is available.

## Security Setup

## reCAPTCHA with OpenLiteSpeed

As of OpenLiteSpeed 1.5.1, reCAPTCHA is available as a method of defense against DDoS attack.

### How To Enable at the Server Level

Access the WebAdmin console via `https://YOUR_SERVER_IP:7080`
Navigate to **Configuration** \> **Server** \> **Security** \>** LS reCAPTCHA**
<https://openlitespeed.org/wp-content/uploads/2019/05/ols-recaptha-sl.png>

  - **Enable reCAPTCHA**: The master switch. This should be set to `Yes`. If you wish to use reCAPTCHA at the virtual host level, then you must first enable it here, at the server level. reCAPTCHA will be activated when the number of concurrent requests reaches the configured **Connection Limit**.
  - **Connection Limit**: The number of concurrent connections (SSL & non-SSL) needed to activate reCAPTCHA. reCAPTCHA will be used until concurrent connections drop below this number. Initially you should set this number low enough for easy testing. For example, `2`.  The default value is `15000`, which makes it almost impossible to activate the reCAPTCHA.
  - **SSL Connection Limit**:  The number of concurrent SSL connections needed to activate reCAPTCHA. reCAPTCHA will be used until concurrent connections drop below this number.  Initially you should set this number low enough for easy testing. For example, `2`.  The default value is `10000`, which makes it almost impossible to activate the reCAPTCHA.
  - **reCAPTCHA Type**: The reCAPTCHA type to use with the key pairs. If a key pair has not been provided and this setting is set to `Not Set`, a default key pair of type `Invisible` will be used. `Checkbox` will display a check box reCAPTCHA for the visitor to validate.  `Invisible` will attempt to validate the reCAPTCHA automatically, and if successful, will redirect to the desired page. `Invisible` is the default, but for easy testing, you can switch to `Checkbox`.
  - **Site Key**: The public key provided by Google via its reCAPTCHA service. A default Site Key will be used if not set.
  - **Secret Key**:  The private key provided by Google via its reCAPTCHA service. A default Secret Key will be used if not set.
  - **Max Tries**: The maximum number of reCAPTCHA attempts permitted before denying the visitor. Default value is `3`.
  - **Allowed Robot Hits**:  Number of hits per 10 seconds to allow "good bots" to pass. Bots will still be throttled when the server is under load. Default value is `3`.
  - **Bot White List**:  List of custom user agents to allow access. Will be subject to the "good bots" limitations, including Allowed Robot Hits.

### How To Enable at the Virtual Host Level

**Tip**: Server-level reCAPTCHA must be enabled, as it is the master switch.
Virtual-host-level connection limits will override server level limits.
Virtual-host-level reCAPTCHA is enabled through the WebAdmin console. (It is not possible to enable reCAPTCHA through Rewrite Rules with OLS. That functionality is currently only available with [LiteSpeed Enterprise](https://www.litespeedtech.com/products/litespeed-web-server).)
Navigate to **Configuration** \> **Virtual Hosts** \> **Security** and set **LS** **reCAPTCHA \> Enable reCAPTCHA** to `Yes`.
<https://openlitespeed.org/wp-content/uploads/2019/05/ols-recaptha-vh.png>

  - **Concurrent Request Limit**:  The number of concurrent requests needed to activate reCAPTCHA. reCAPTCHA will be used until concurrent requests drop below this number. Initially you should set this number low enough for easy testing. For example, `2`.  The default value is `15000`, which makes it almost impossible to activate the reCAPTCHA.
  - **reCAPTCHA Type**: The reCAPTCHA type to use with the key pairs. If a key pair has not been provided and this setting is set to `Not Set`, a default key pair of type `Invisible` will be used. `Checkbox` will display a check box reCAPTCHA for the visitor to validate.  `Invisible` will attempt to validate the reCAPTCHA automatically, and if successful, will redirect to the desired page. `Invisible` is the default, but for easy testing, you can switch to `Checkbox`.
  - **Max Tries**: The maximum number of reCAPTCHA attempts permitted before denying the visitor. Default value is `3`.

### Customizing the Good Bots List

Google bots are considered good bots because they help index your site. However, they cannot do their job properly without receiving the correct page. The **Bot White List** configuration may be used to specify bots that you may need for your site.
![](https://openlitespeed.org/wp-content/uploads/2019/05/ols-recap3.jpg)
Here, we have configured `Edge` in the **Bot White List** text area. Bot White List is a contains match, but regex may be used as well.
After restarting, browsers containing `Edge` in the user-agent header will bypass reCAPTCHA:
![](https://openlitespeed.org/wp-content/uploads/2019/05/ols-recap4.png)
The browser on the left is Microsoft Edge, the browser on the right is Chrome.}}
The **Allowed Robot Hits** configuration may be used to limit how many times a good bot (including Googlebot) is allowed to hit a URL before it is redirected to reCAPTCHA as well. This may be useful to prevent bad actors from bypassing reCAPTCHA using a custom user agent.

### Customizing the reCAPTCHA Page

The default reCAPTCHA page is generic. If you would like to customize the page, you may do so by creating a file at `$SERVER_ROOT/lsrecaptcha/_recaptcha_custom.shtml`
There are two script tags that are required and it is strongly recommended to avoid changing the `form` and the `recaptchadiv` unless you know what you are doing. There are three echos within the page itself. Those are used by the web server to customize the reCAPTCHA type and keys and specify any query string used.
Beyond those required attributes, everything else is customizable. As noted before, please ensure that you have backups of the default page and your customized page. Note that the `.shtml` extension is required in order to use configured type and keys.

### Apply Your Own Site Key

You can apply your own reCAPTCHA key and adjust the configuration as you like. Client verification is completely determined by Google's reCAPTCHA service. The invisible type may display a difficult puzzle.
For server wide protection that needs to cover a lot of domains, make sure **Verify the origin of reCAPTCHA** solutions is unchecked. Otherwise, you may need to apply a key for each domain.

### reCAPTCHA Returning 403 and Dropping Connection

If reCAPTCHA fails a few times, it will return a 403 error and then drop the connection from that IP. It is the way it works in order to block attacks. If the `invisible` reCAPTCHA keeps auto-refreshing and then fails, just change the type to `one-click`


## Per-Client Throttling

OpenLiteSpeed includes a built-in Per-Client Throttling feature which allows you to block bad IPs.
Navigate to **Configuration \> Server \> Security configurations \> Per Client Throttling** to find several configuration settings that you can use to limit the request, bandwidth, and connection rate per remote IP address.

### Request Throttling

Separate controls are available for throttling requests for static files and dynamic content.

### Bandwidth Throttling

The server allows setting separate bandwidth limits for inbound and outbound traffic.
Bandwidth numbers will be rounded up in 4KB increments.
Set to `0` to disable throttling.
The **Outbound Bandwidth** limit allows serving more unique clients and prevents limited network bandwidth from getting used up by a small number of clients with fast network connections.

### Connection Throttling

These settings control concurrent connections coming from one client (IP address) and guard against DoS attacks.

**Connection Hard Limit** controls how many concurrent connections are allowed from one IP address. If an IP reaches the hard connection limit, the web server will immediately close newly accepted connections from that IP address, and move on to pending connections from different IP addresses. As almost all web browsers support keep-alive/persistent connections (multiple requests pipelined through one connection), the number of connections required in normal browsing is very small. Typically, one connection is enough, but some web browsers try to establish additional connections to speed up downloading. Allowing 4 to 10 connections from one IP is recommended. Less than that will probably affect normal web services.

Use **Connection Soft Limit**, **Grace Period**, and Banned Period to spot and mitigate abusers: An IP address that stays over the soft limit for the length of the grace period will be banned for the length of time set in **Banned Period**. This is a good way to identify IPs that should be added to the **Denied List**.

**Note**: The number of connections can temporarily exceed the soft limit during the grace period, as long as it is under the hard limit. After the grace period, if it is still above the soft limit, then no more connections will be allowed from that IP for duration of the banned period.

### Example

Default Settings:
![](https://openlitespeed.org/wp-content/uploads/2019/12/ols-throttle1.jpg)
Updated Settings:
![](https://openlitespeed.org/wp-content/uploads/2019/12/ols-throttle2.jpg)
**Static Requests/second** = `40`
**Dynamic Requests/second** = `2`
**Outbound Bandwidth (bytes/sec)** = `0`
**Inbound Bandwidth (bytes/sec)** = `0`
**Connection Soft Limit** = `15`
**Connection Hard Limit** = `20`
**Block Bad Request** = `Yes`
**Grace Period (sec)** = `15`
**Banned Period (sec)** = `60`
Explanation: An IP that has established more than 20 connections with the web server, or has established over 15 connections of over 15 seconds (the grace period), is treated as a DoS-attacker. The server will ban the IP for 60 seconds and record a log entry in the error log file. To exclude any IP from the client throttle limits (and bypass DDoS detection), add the IP with a trailing `T` (aka trusted) in **Allowed List** (**WebAdmin Console \> Server \> Security \> Access Control**).
The hard limit can be adjusted based on an attacker's strategy. If the botnet is not very aggressive, you will need to lower the limit to just below their max connection per IP, to make sure it won't affect a regular user. If they only make very few connections per IP, do not use the hard limit to detect them.



## Proxy Setup 
