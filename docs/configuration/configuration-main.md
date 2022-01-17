---
layout: default
title: Main Configuration
nav_order: 3
permalink: /docs/configuration/main
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
{: .no_toc}
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
  - **Virtual Host Name** = `Example2`
  - **Virtual Host Root** = `$SERVER_ROOT/Example2`
  - **Config File** = `$SERVER_ROOT/conf/vhosts/Example2/vhost.conf`
  - **Enable Scripts/ExtApps** = `Yes` 
  - **Restrained** = `No`
4. You might see "file /usr/local/lsws/conf/vhosts/Example2/vhost.conf does not exist. CLICK TO CREATE" alert due to we starting from scratch, click the CLICK TO CREATE button so OpenLiteSpeed will make one for me. 
5. Click save button, go back into the Example2 virtual host's configurations, and specify the document root under the General tab
  - **Document Root** = `/usr/local/lsws/Example2/html`
  - **Index Files** = `index.html, index.php`
6. I'd also recommend you to enable the Rewrite feature which is under the Rewrite tab
  - **Enable Rewrite** = `Yes`
  - **Auto Load from .htaccess** = `yes`


## Listeners setup
### Listener create 
Go to the WebAdmin console > Listeners. The default listener that listens to all IPs on port 8088. You might want to create another two listeners for both HTTP and HTTPS ports.  
1. Click the Add button to create a HTTP listener with following settings:
  - **Listener Name** = `HTTP`
  - **IP Address** = `ANY IPv4`
  - **Port** = `80`
  - **Secure** = `No`
2. Click the Add button to create a HTTPS listener with following settings:
  - **Listener Name** = `HTTPS`
  - **IP Address** = `ANY IPv4`
  - **Port** = `443`
  - **Secure** = `Yes`
3. Navigate  to the WebAdmin Console > Listeners > HTTPS (or whatever name you used) > SSL, and click the Edit button. Specify the private key and certificate path, then click the Save button.
  Let's Encrypt
  - **Private Key File** = `/etc/letsencrypt/live/example.com/privkey.pem` (Use your own key path)
  - **Certificate File** = `/etc/letsencrypt/live/example.com/fullchain.pem` (Use your own cert path)
  - **Chained Certificate** = `Yes`
  Temporary certificate(web admin) or [private self-signed certificate](https://stackoverflow.com/questions/10175812/how-to-create-a-self-signed-certificate-with-openssl)
  - **Private Key File** = `/usr/local/lsws/conf/example.key` (Use your own key path)
  - **Certificate File** = `/usr/local/lsws/conf/example.crt` (Use your own cert path)
  - **Chained Certificate** = `Not Set`

### Virtual Hosts mapping
Go to the WebAdmin console > Listeners > HTTP, click the Add button from the Virtual Host Mappings area and set with following settings. 
  - **Virtual Host** = `Example2`
  - **Domains** = `example2.com, www.example2.com`
Go to the WebAdmin console > Listeners > HTTPS, click the Add button from the Virtual Host Mappings area and set with following settings. 
  - **Virtual Host** = `Example2`
  - **Domains** = `example2.com, www.example2.com`

## SSL setup

OpenLiteSpeed supports Server Name Indication (SNI), allowing users to set SSL certificates at the virtual host level. If you have additional virtual host with a SSL certificate,

1. Add the certificate in the **WebAdmin Console**: Navigate to **Virtual Hosts > "new VH" > SSL > Edit SSL Private Key & Certificate**
2. Give the new SSL settings with the following example as a guide:
  - **Private Key File** = `/usr/local/lsws/conf/example.key` (Use your own key path)
  - **Certificate File** = `/usr/local/lsws/conf/example.crt` (Use your own cert path)
  - **Chained Certificate** = `Not Set`

Note 1: No need to set an addition listener for different domain or SSL
Note 2: Even you have SSL set in the VH, you do not want to leave the port 443 listener' SSL empty

## Cache Setup
Cache Module is Installed and Enabled by default, so we don't need to change any settings for it. 

You still need to enable caching for your web apps, and this can be done by [installing the corresponding LSCache plugin](https://docs.litespeedtech.com/lscache/#available-plugins), or using [rewrite rules](https://docs.litespeedtech.com/lscache/noplugin/) in .htaccess if no plugin is available.


