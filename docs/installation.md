---
layout: default
title: Installation
nav_order: 2
permalink: /installation
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


## Install from LiteSpeed Repository

### Install LiteSpeed Repository
{: .no_toc}

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

### Install OpenLiteSpeed
{: .no_toc}

For Debian/Ubuntum,
```bash
sudo apt-get install openlitespeed
```
For CentOS,
```bash
sudo yum install openlitespeed
```

### Install PHP
{: .no_toc}

internal url


## Install from One-Click Script

To install OpenLiteSpeed, LSPHP, MariaDB, WordPress, and LiteSpeed Cache plugin,
```bash
bash <( curl -k https://raw.githubusercontent.com/litespeedtech/ols1clk/master/ols1clk.sh ) -w
```
To install OpenLiteSpeed and lsphp, 
```bash
bash <( curl -k https://raw.githubusercontent.com/litespeedtech/ols1clk/master/ols1clk.sh )
```
More examples

| Examples                             | Description                                                                         |
| ------------------------------------ | ----------------------------------------------------------------------------------- |
| `./ols1clk.sh`                       | To install OpenLiteSpeed with a random WebAdmin password.                           |
| `./ols1clk.sh --lsphp 80`            | To install OpenLiteSpeed with lsphp80.                                              |
| `./ols1clk.sh -A 123456 -e a@cc.com` | To install OpenLiteSpeed with WebAdmin password "123456" and email <a@cc.com>.      |
| `./ols1clk.sh -R 123456 -W`          | To install OpenLiteSpeed with WordPress and MySQL root password "123456".           |
| `./ols1clk.sh --wordpressplus a.com` | To install OpenLiteSpeed with a fully configured WordPress installation at "a.com". |

## Launch from Existing Image

|   |[<img src="/docs/assets/images/Cloud/wp_50.svg" width = "100">](https://docs.litespeedtech.com/cloud/wordpress/)|[<img src="/docs/assets/images/Cloud/cyberpanel_50.svg" width = "100">](https://docs.litespeedtech.com/cloud/cyberpanel/) |[<img src="/docs/assets/images/Cloud/django_50.svg" width = "100">](https://docs.litespeedtech.com/cloud/django/) | [<img src="/docs/assets/images/Cloud/nodejs_50.svg" width = "100">](https://docs.litespeedtech.com/cloud/nodejs/)|[<img src="/docs/assets/images/Cloud/ruby_50.svg" width = "100">](https://docs.litespeedtech.com/cloud/rails/)|
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
||[WordPress](https://docs.litespeedtech.com/cloud/wordpress/)|[CyberPanel](https://docs.litespeedtech.com/cloud/cyberpanel/)|[Django](https://docs.litespeedtech.com/cloud/django/)|[NodeJS](https://docs.litespeedtech.com/cloud/nodejs/)|[Rails](https://docs.litespeedtech.com/cloud/rails/)|
| [**DigitalOcean**](https://marketplace.digitalocean.com/category/blogs-and-forums)  | [Launch](https://cloud.digitalocean.com/droplets/new?image=litespeedtechnol-openlitespeedwor-18-04&utm_source=openlitespeed&utm_campaign=openlitespeed-wp)  | [Launch](https://cloud.digitalocean.com/droplets/new?image=cyberpanel-18-04&utm_source=cyberpanel&utm_campaign=cyberpanel) | [Launch](https://cloud.digitalocean.com/droplets/new?image=openlitespeed-django-18-04&utm_source=openlitespeed&utm_campaign=openlitespeed-django) | [Launch](https://cloud.digitalocean.com/droplets/new?image=openlitespeed-node-18-04&utm_source=openlitespeed&utm_campaign=openlitespeed-node) | [Launch](https://cloud.digitalocean.com/droplets/new?image=litespeedtechnol-openlitespeedrai-20-04&utm_source=openlitespeed&utm_campaign=openlitespeed-rails) |
|[**GCP**](https://console.cloud.google.com/marketplace/browse?q=litespeed)|[Launch](https://console.cloud.google.com/marketplace/details/gc-image-pub/openlitespeed-wordpress)| [Launch](https://console.cloud.google.com/marketplace/details/gc-image-pub/cyberpanel) | [Launch](https://console.cloud.google.com/marketplace/details/gc-image-pub/openlitespeed-django) | [Launch](https://console.cloud.google.com/marketplace/details/gc-image-pub/openlitespeed-nodejs) |[Launch](https://console.cloud.google.com/marketplace/details/gc-image-pub/openlitespeed-rails)|
|[**AWS**](https://aws.amazon.com/marketplace/search/results?x=0&y=0&searchTerms=litespeed)|[Launch](https://aws.amazon.com/marketplace/pp/B07KSC2QQN)|[Launch](https://aws.amazon.com/marketplace/pp/B07MPZQ4PS)|[Launch](https://aws.amazon.com/marketplace/pp/B07MZ6VVRD)|[Launch](https://aws.amazon.com/marketplace/pp/B07MZ393TM)|[Launch](http://aws.amazon.com/marketplace/pp/B08JVDJQ1L)|
|[**Azure**](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=litespeed)|[Launch](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/litespeedtechnologies.openlitespeed-wordpress)|[Launch](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/litespeedtechnologies.cyberpanel)|[Launch](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/litespeedtechnologies.openlitespeed-django)|[Launch](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/litespeedtechnologies.openlitespeed-nodejs)|[Launch](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/litespeedtechnologies.openlitespeed-rails)|
|[**Alibaba**](https://marketplace.alibabacloud.com/)|[Launch](https://marketplace.alibabacloud.com/products/56720001/OpenLiteSpeed_WordPress_em_-sgcmjj00024846.html)|[Launch](https://marketplace.alibabacloud.com/products/56720001/sgcmjj00024863.html)|[Launch](https://marketplace.alibabacloud.com/products/56720001/OpenLiteSpeed_Django-sgcmjj00024874.html)|[Launch](https://marketplace.alibabacloud.com/products/56720001/sgcmjj00024862.html)|[Launch](https://marketplace.alibabacloud.com/products/56720001/sgcmjj00024972.html)|
|[**Linode**](https://www.linode.com/marketplace/apps/?sq=litespeed)|[Launch](https://www.linode.com/marketplace/apps/litespeed-technologies/openlitespeed-wordpress/)|[Launch](https://www.linode.com/marketplace/apps/litespeed-technologies/cyberpanel/)|[Launch](https://cloud.linode.com/stackscripts/458602)|[Launch](https://cloud.linode.com/stackscripts/458633)|[Launch](https://cloud.linode.com/stackscripts/641872)|
|[**Vultr**](https://www.vultr.com/marketplace/)|[Launch](https://www.vultr.com/marketplace/apps/openlitespeed-wordpress)|[Launch](https://www.vultr.com/marketplace/apps/cyberpanel)|[Launch](https://www.vultr.com/marketplace/apps/openlitespeed-django)|[Launch](https://www.vultr.com/marketplace/apps/openlitespeed-nodejs)|[Launch](https://www.vultr.com/marketplace/apps/openlitespeed-rails)|


## Launch from Docker

Launch an Existing Image from [Docker Hub](https://hub.docker.com/search?q=litespeedtech&type=image)

||![ols](/docs/assets/images/Cloud/docker-ols-logo_160x160.png)|
| :-------------: | :-------------: |
|Server Only|OpenLiteSpeed<br>[Instructions](https://docs.litespeedtech.com/cloud/docker/openlitespeed/)<br>[Launch](https://hub.docker.com/repository/docker/litespeedtech/openlitespeed)|
|Server + WordPress|OLS+WP<br>[Instructions](https://docs.litespeedtech.com/cloud/docker/ols%2Bwordpress/)<br>[Launch](https://hub.docker.com/repository/docker/litespeedtech/openlitespeed)|