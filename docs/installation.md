---
layout: default
title: Installation
nav_order: 2
permalink: /installation
---

1. TOC
{:toc}

---

### Supported OS 
{: .no_toc .text-delta }
- [Debian 7, 8, 9, 10](https://www.debian.org/distrib/)
- [Ubuntu 16, 18, 20](https://www.ubuntu.com/download)
- [CentOS 6, 7, 8](https://www.centos.org/download/)
- [AlamaLinux 8](https://mirrors.almalinux.org/isos.html)


## Install from LiteSpeed Repository

For Debian/Ubuntum, run the following command to install LiteSpeed Repository
```
sudo wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | sudo bash
```
For CentOS, run the following command to install LiteSpeed Repository

AlmaLinux 8 & CentOS 8: 
```
rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el8.noarch.rpm
```
CentOS 7:
```
rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el7.noarch.rpm
```

### Install OpenLiteSpeed
{: .no_toc .text-delta }
For Debian/Ubuntum,
```
apt-get install openlitespeed
```
For CentOS,
```
yum install epel-release
yum install openlitespeed
```

### Install PHP
{: .no_toc .text-delta }
internal url


## Install from One-Click Script
To install OpenLiteSpeed, LSPHP, MariaDB, WordPress, and LiteSpeed Cache plugin,
```
bash <( curl -k https://raw.githubusercontent.com/litespeedtech/ols1clk/master/ols1clk.sh ) -w
```
To install OpenLiteSpeed and lsphp, 
```
bash <( curl -k https://raw.githubusercontent.com/litespeedtech/ols1clk/master/ols1clk.sh )
```
### Options
{: .no_toc .text-delta }
| Opt  | Options                        | Description                                                                                 |
| :--: | ------------------------------ | ------------------------------------------------------------------------------------------- |
| `-A` | `--adminpassword [PASSWORD]`   | To set the WebAdmin password for OpenLiteSpeed instead of using a random one.               |
| `-E` | `--email [EMAIL]`              | To set the administrator email.                                                             |
|      | `--lsphp [VERSION]`            | To set the LSPHP version, such as 80. We currently support versions '56 70 71 72 73 74 80'. |
|      | `--mariadbver [VERSION]`       | To set MariaDB version, such as 10.5. We currently support versions '10.2 10.3 10.4 10.5'.  |
| `-W` | `--wordpress`                  | To install WordPress. You will still need to complete the WordPress setup by browser        |
|      | `--wordpressplus [SITEDOMAIN]` | To install, setup, and configure WordPress, also LSCache will be enabled                    |
|      | `--wordpresspath [WP_PATH]`    | To specify a location for the new WordPress installation or use for an existing WordPress.  |
| `-R` | `--dbrootpassword [PASSWORD]`  | To set the database root password instead of using a random one.                            |
|      | `--dbname [DATABASENAME]`      | To set the database name to be used by WordPress.                                           |
|      | `--dbuser [DBUSERNAME]`        | To set the WordPress username in the database.                                              |
|      | `--dbpassword [PASSWORD]`      | To set the WordPress table password in MySQL instead of using a random one.                 |
|      | `--listenport [PORT]`          | To set the HTTP server listener port, default is 80.                                        |
|      | `--ssllistenport [PORT]`       | To set the HTTPS server listener port, default is 443.                                      |
|      | `--wpuser [WP_USER]`           | To set the WordPress admin user for WordPress dashboard login. Default value is wpuser.     |
|      | `--wppassword [PASSWORD]`      | To set the WordPress admin user password for WordPress dashboard login.                     |
|      | `--wplang [WP_LANGUAGE]`       | To set the WordPress language. Default value is "en\_US" for English.                       |
|      | `--sitetitle [WP_TITLE]`       | To set the WordPress site title. Default value is mySite.                                   |
| `-U` | `--uninstall`                  | To uninstall OpenLiteSpeed and remove installation directory.                               |
| `-P` | `--purgeall`                   | To uninstall OpenLiteSpeed, remove installation directory, and purge all data in MySQL.     |
| `-Q` | `--quiet`                      | To use quiet mode, won't prompt to input anything.                                          |
| `-V` | `--version`                    | To display the script version information.                                                  |
| `-v` | `--verbose`                    | To display more messages during the installation.                                           |
|      | `--update`                     | To update ols1clk from github.                                                              |
| `-H` | `--help`                       | To display help messages.                                                                   |

| Examples                             | Description                                                                         |
| ------------------------------------ | ----------------------------------------------------------------------------------- |
| `./ols1clk.sh`                       | To install OpenLiteSpeed with a random WebAdmin password.                           |
| ` ./ols1clk.sh --lsphp 80  `         | To install OpenLiteSpeed with lsphp80.                                              |
| `./ols1clk.sh -A 123456 -e a@cc.com` | To install OpenLiteSpeed with WebAdmin password "123456" and email <a@cc.com>.      |
| ` ./ols1clk.sh -R 123456 -W  `       | To install OpenLiteSpeed with WordPress and MySQL root password "123456".           |
| `./ols1clk.sh --wordpressplus a.com` | To install OpenLiteSpeed with a fully configured WordPress installation at "a.com". |

## Install from Cloud Images
[To get high performance web servers with applications including WordPress, Django, NodeJS, Rails, Control Panels and more](https://docs.litespeedtech.com/cloud/images/)
## Install from Docker
[Launch an Existing Image from Docker Hub](https://docs.litespeedtech.com/cloud/docker/)