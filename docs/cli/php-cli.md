---
layout: default
title: PHP Setup With CLI
nav_order: 2
parent: CLI
permalink: /docs/cli/cli
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

## Set Up LSPHP at the Server Level From the Command Line

### Define External Applications for PHPs

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

### Set up Script Handlers 

Add the following to the `httpd_config.conf` file:

```bash
scriptHandler{
    add lsapi:lsphp80  php
}
```
