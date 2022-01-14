---
layout: default
title: Install from Repository
parent: Installation
nav_order: 1
permalink: /docs/installation/repo
---

# Install from LiteSpeed Repository

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}

</details>

## Supported OS 

You can install OpenLiteSpeed via the LiteSpeed Repo on any of these operating systems:

- [Debian 7, 8, 9, 10](https://www.debian.org/distrib/)
- [Ubuntu 16, 18, 20](https://www.ubuntu.com/download)
- [CentOS 6, 7, 8](https://www.centos.org/download/)
- [AlamaLinux 8](https://mirrors.almalinux.org/isos.html)

## Install LiteSpeed Repository

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

## Next Steps
{: .no_toc} 

Once you've installed OpenLiteSpeed itself, you will want to set up PHP and otherwise configure the server. See the following for instructions:

1. [PHP](/docs/php)
2. [Configuration](/docs/configuration)
