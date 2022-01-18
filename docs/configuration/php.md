---
layout: default
title: Setup PHP
nav_order: 3
permalink: /php
---

# Set Up LSPHP With the LiteSpeed Repository
{: .no_toc}

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}

</details>

## Install LSPHP

The easiest way to get up and running with PHP is to use the LiteSpeed Repository. The LiteSpeed Repository comes with prebuilt PHP packages, all of which have LiteSpeed support built in.

### Install LiteSpeed Repository, if Necessary
{: .no_toc}

**NOTE**: If you already installed the LiteSpeed Repository when you Installed OpenLiteSpeed, you can skip this step.

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

### Install LSPHP
{: .no_toc}

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

## Set up PHP at the Server Level

### Define External Applications for PHPs
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

### Set up Script Handlers 
{: .no_toc}

1. Add any script handlers in the **WebAdmin Console**: Navigate to **Server Configuration > Script handler > Add**
2. Give the new script handler appropriate settings, using the following example as a guide:
  - **Suffixes**: `php`
  - **Handler Type**: `LiteSpeed SAPI`
  - **Handler Name**: `lsphp80`

## Set up PHP at the Virtual Host Level 
If you want to use different settings for any of your virtual hosts, you can set up the same external applications and script handlers at the virtual host level, which will override any server-level script handler settings. 
1. Add a virtual host-level script handler in the **WebAdmin Console**: Navigate to **Server Configuration > Virtual Hosts > [Example] > Script Handlers > Add**, replacing "[Example]" with the name of the actual Virtual Host you want to edit.
2. Give the new script handler appropriate settings, using the following example as a guide:
  - **Suffixes**: `php`
  - **Handler Type**: `LiteSpeed SAPI`
  - **Handler Name**: `lsphp80`
