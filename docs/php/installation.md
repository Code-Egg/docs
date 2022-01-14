---
layout: default
title: Installation
parent: PHP
nav_order: 2
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

### Supported OS 
{: .no_toc}
- [Debian 7, 8, 9, 10](https://www.debian.org/distrib/)
- [Ubuntu 16, 18, 20](https://www.ubuntu.com/download)
- [CentOS 6, 7, 8](https://www.centos.org/download/)
- [AlamaLinux 8](https://mirrors.almalinux.org/isos.html)

### Install LiteSpeed Repository
{: .no_toc}

The easiest way to get up and running with PHP is to use the LiteSpeed Repository. The LiteSpeed Repository comes with prebuilt PHP packages with LiteSpeed support built in.

Before you can install LSPHP there are a couple of prerequisite packages and repositories that need to be installed via the operating system's package manager. This can be accomplished by running the following commands in a terminal:

- Debian/Ubuntum
```
sudo wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | sudo bash
```
- AlmaLinux 8 & CentOS 8:
```
sudo rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el8.noarch.rpm
sudo yum install epel-release
```
- CentOS 7:
```
sudo rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el7.noarch.rpm
sudo yum install epel-release
```

### Install LSPHP
{: .no_toc}

For Debian/Ubuntum,
```
sudo apt-get install lsphp80 lsphp80-common lsphp80-mysql
```
For CentOS,
```
sudo yum install lsphp80 lsphp80-common lsphp80-mysqlnd
```

This will install `lsphp80` and `lsphp80-mysql` into the following location: `/usr/local/lsws/lsphp80/bin/lsphp`.

To find out all the LSPHP packages and extensions we offer you can run the following:
For Debian/Ubuntum,
```
sudo apt-cache search lsphp
```
For CentOS,
```
sudo yum search lsphp
```
