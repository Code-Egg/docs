---
layout: default
title: Installation
nav_order: 2
has_children: true
permalink: /installation
---

### Supported OS 
{: .no_toc}
- [Debian 7, 8, 9, 10](https://www.debian.org/distrib/)
- [Ubuntu 16, 18, 20](https://www.ubuntu.com/download)
- [CentOS 6, 7, 8](https://www.centos.org/download/)
- [AlamaLinux 8](https://mirrors.almalinux.org/isos.html)


## Install from LiteSpeed Repository

### Install LiteSpeed Repository
{: .no_toc}

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

### Install OpenLiteSpeed
{: .no_toc}

- Debian/Ubuntu
```bash
sudo apt-get install openlitespeed
```
- CentOS
```bash
sudo yum install openlitespeed
```

### Next step
{: .no_toc} 

You might want to install LSPHP and configure the server settings. 
1. [LSPHP](/docs/php)
2. [Configuration](/docs/configuration)

## Other fast setup method