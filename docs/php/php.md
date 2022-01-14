---
layout: default
title: PHP
nav_order: 4
has_children: true
permalink: /php
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}

</details>


## Install LSPHP

### Install LiteSpeed Repository
{: .no_toc}

The easiest way to get up and running with PHP is to use the LiteSpeed Repository. The LiteSpeed Repository comes with prebuilt PHP packages with LiteSpeed support built in.

- Debian/Ubuntum
```bash
sudo wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | sudo bash
```
- AlmaLinux 8 & CentOS 8:
```bash
sudo rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el8.noarch.rpm
sudo yum install epel-release
```
- CentOS 7:
```bash
sudo rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el7.noarch.rpm
sudo yum install epel-release
```

### Install LSPHP
{: .no_toc}

- Debian/Ubuntu
```bash
sudo apt-get install lsphp80 lsphp80-common lsphp80-mysql
```
- CentOS
```bash
sudo yum install lsphp80 lsphp80-common lsphp80-mysqlnd
```

This will install `lsphp80` and `lsphp80-mysql` into the following location: `/usr/local/lsws/lsphp80/bin/lsphp`.

To find out all the LSPHP packages and extensions we offer you can run the following:

- Debian/Ubuntu
```bash
sudo apt-cache search lsphp
```
- CentOS
```bash
sudo yum search lsphp
```

## Setup PHP at server level
### Define External Applications for PHPs
We will go through creating an external application for PHP 8.0. However, you can also use the default external applications with some modification. 
1. Add an external application (in the WebAdmin console > Server Configuration > External App > Add).
2. Choose `LiteSpeed SAPI App` for the application type
3. Give the new external application an appropriate name, address, maximum number of connections, initial,
  - Name= `lsphp80`
  - Address= `uds://tmp/lshttpd/lsphp80.sock`
  - Max Connections= `10`
  - Environment= ```PHP_LSAPI_CHILDREN=10
             LSAPI_AVOID_FORK=200M```
  - Initial Request Timeout (secs) = `60`
  - Persistent Connection = `Yes`
  - Start By Server = `1` 
  - Command = `lsphp80/bin/lsphp`
  - Back Log = `100`
  - Instances = `1`
  - Memory Soft Limit (bytes) = `2047M`
  - Memory Hard Limit (bytes) = `2047M`
  - Process Soft Limit = `1400`
  - Process Hard Limit = `1500`

### Set up Script Handlers 
1. Add an Script Handlers  (in the WebAdmin console > Server Configuration > Script handler > Add).
2. Give the new Script handler an appropriate Suffixes, type and Name,
  - Suffixes = `php`
  - Handler Type = `LiteSpeed SAPI`
  - Handler Name = lsphp80

## Setup PHP at virtual host level 
We can setup the same External Applications and Script Handlers at virtual host level, this will override any server-level script handler settings. 
1. Add a virtual host-level script handler (WebAdmin console > Server Configuration > Virtual Hosts > Example > Script Handlers > Add).
2. Give the new Script handler an appropriate Suffixes, type and Name,
  - Suffixes = `php`
  - Handler Type = `LiteSpeed SAPI`
  - Handler Name = lsphp80
