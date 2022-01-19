---
layout: default
title: Install from Repository
parent: Installation
nav_order: 1
permalink: /docs/installation/repo
---

# Install OpenLiteSpeed and PHP from LiteSpeed Repository
{: .no_toc} 

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}

</details>

## Install the LiteSpeed Repository

- Debian/Ubuntu
```bash
sudo wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | sudo bash
```
- AlmaLinux 8 & CentOS 8
```bash
sudo rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el8.noarch.rpm
sudo yum install epel-release
```
- CentOS 7
```bash
sudo rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el7.noarch.rpm
sudo yum install epel-release
```

## Install OpenLiteSpeed

- Debian/Ubuntu
```bash
sudo apt-get install openlitespeed
```
- CentOS
```bash
sudo yum install openlitespeed
```

## WebAdmin Access

Run the following command to set the WebAdmin password if needed:
```bash
sudo /usr/local/lsws/admin/misc/admpass.sh
```
To access the WebAdmin console, visit port `7080` of your domain (for example,`https://example.com:7080/`) and log in using the password you just set. 

## Install LSPHP

This command will install `lsphp80` and `lsphp80-mysql` into `/usr/local/lsws/lsphp80/bin/lsphp`:

- Debian/Ubuntu
```bash
sudo apt-get install lsphp80 lsphp80-common lsphp80-mysql
```
- CentOS
```bash
sudo yum install lsphp80 lsphp80-common lsphp80-mysqlnd
```

To get a list of the LSPHP packages and extensions available, you can run the following:

- Debian/Ubuntu
```bash
sudo apt-cache search lsphp
```
- CentOS
```bash
sudo yum search lsphp
```

## Set up PHP

### Method 1: WebAdmin Console

#### Define External Applications at the Server Level
{: .no_toc}

This example creates an external application for PHP 8.0, however, you can use the default external applications with some modification, if you prefer. 

1. Add an external application in the **WebAdmin Console**: Navigate to **Server Configuration > External App > Add**
2. Choose `LiteSpeed SAPI App` for the application type
3. Give the new external application an appropriate name, address, maximum number of connections, etc., using the following example as a guide:
  - **Name**: `lsphp80`
  - **Address**: `uds://tmp/lshttpd/lsphp80.sock`
  - **Max Connections**: `10`
  - **Environment**: 
  ```PHP_LSAPI_CHILDREN=10
LSAPI_AVOID_FORK=200M```
  - **Initial Request Timeout (secs)**: `60`
  - **Persistent Connection**: `Yes`
  - **Start By Server**: `1` 
  - **Command**: `lsphp80/bin/lsphp`
  - **Back Log**: `100`
  - **Instances**: `1`
  - **Memory Soft Limit (bytes)**: `2047M`
  - **Memory Hard Limit (bytes)**: `2047M`
  - **Process Soft Limit**: `1400`
  - **Process Hard Limit**: `1500`

#### Set up Script Handlers at the Server Level 
{: .no_toc}

1. Add any script handlers in the **WebAdmin Console**: Navigate to **Server Configuration > Script handler > Add**
2. Give the new script handler appropriate settings, using the following example as a guide:
  - **Suffixes**: `php`
  - **Handler Type**: `LiteSpeed SAPI`
  - **Handler Name**: `lsphp80`

#### Set up PHP at the Virtual Host Level 
{: .no_toc}
If you want to use different settings for any of your virtual hosts, you can set up the same external applications and script handlers at the virtual host level, which will override any server-level script handler settings. 
1. Add a virtual host-level script handler in the **WebAdmin Console**: Navigate to **Server Configuration > Virtual Hosts > [Example] > Script Handlers > Add**, replacing "[Example]" with the name of the actual Virtual Host you want to edit.
2. Give the new script handler appropriate settings, using the following example as a guide:
  - **Suffixes**: `php`
  - **Handler Type**: `LiteSpeed SAPI`
  - **Handler Name**: `lsphp80`

### Method 2: Command Line Interface 
{: .d-inline-block }
Advanced
{: .label .label-yellow }

#### Define External Applications for PHPs
{: .no_toc}
Edit the `httpd_config.conf` file as follows:

```bash
extProcessor lsphp80{
    type                            lsapi
    address                         uds://tmp/lshttpd/lsphp.sock
    maxConns                        35
    env                             PHP_LSAPI_CHILDREN=35
    env                             LSAPI_AVOID_FORK=200M
    initTimeout                     60
    retryTimeout                    0
    persistConn                     1
    pcKeepAliveTimeout
    respBuffer                      0
    autoStart                       1
    path                            lsphp80/bin/lsphp
    backlog                         100
    instances                       1
    priority                        0
    memSoftLimit                    2047M
    memHardLimit                    2047M
    procSoftLimit                   1400
    procHardLimit                   1500
}
```

#### Set up Script Handlers 
{: .no_toc}
Add the following to the `httpd_config.conf` file:

```bash
scriptHandler{
    add lsapi:lsphp80  php
}
```

## Next Step
{: .no_toc} 

- [Configuration](/docs/configuration)
