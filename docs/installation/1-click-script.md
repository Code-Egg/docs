---
layout: default
title: 1-click script
parent: Installation
nav_order: 3
---

# 1-Click Install OLS, PHP, MySQL, WP and LSCache

You can install OpenLiteSpeed, LSPHP, MariaDB, WordPress, and LiteSpeed Cache plugin for WordPress in 1-click\!
`ols1clk` is a one-click installation script for OpenLiteSpeed. Using this script, you can quickly and easily install OpenLiteSpeed with its default settings, Using different parameters for example, `./ols1clk.sh -w` you can install WordPress files and MySQL database associated with it, along with [LiteSpeed Cache Plugin](https://wordpress.org/plugins/litespeed-cache/) for WordPress at the same time.

## Requirements

**Currently one-click installation only supports Centos(6-8), Debian(7-10) and Ubuntu(14,16,18,20) with 64 bits platforms**

  - **Operating System:**
      - [CentOS 6, 7, 8](https://www.centos.org/download/)
      - [Debian 7, 8, 9, 10](https://www.debian.org/distrib/)
      - [Ubuntu 14,16,18,20](https://www.ubuntu.com/download)

<!-- end list -->

  - **User Permission:**
      - `ols1clk` be run with superuser access, You can either switch to the super user (root) with the `su` command *or* (you may run it as root or use the `sudo` command). How you do this will depend upon which distribution you use. Some distributions enable the root user (such as CentOS), while some do not (such as Ubuntu and Debian).

## Installation

  - Run the following in the command line:

<!-- end list -->

    wget https://raw.githubusercontent.com/litespeedtech/ols1clk/master/ols1clk.sh && bash ols1clk.sh

**OR**

  - Run the following in the command line:

<!-- end list -->

``` line-numbers
bash <( curl -k https://raw.githubusercontent.com/litespeedtech/ols1clk/master/ols1clk.sh )
```

  - The above methods will install OpenLiteSpeed and lsphp module.

  - To install WordPress and (if not already present) along with MySQL database, along with [LiteSpeed Cache Plugin](https://wordpress.org/plugins/litespeed-cache/) for WordPress, run
    
        bash <( curl -k https://raw.githubusercontent.com/litespeedtech/ols1clk/master/ols1clk.sh ) -w

  - Answer any prompts within the script and you're done\!

## Options

    ./ols1clk.sh [options] [options] …

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

### [](#examples)Examples

| Examples                             | Description                                                                         |
| ------------------------------------ | ----------------------------------------------------------------------------------- |
| `./ols1clk.sh`                       | To install OpenLiteSpeed with a random WebAdmin password.                           |
| ` ./ols1clk.sh --lsphp 80  `         | To install OpenLiteSpeed with lsphp80.                                              |
| `./ols1clk.sh -A 123456 -e a@cc.com` | To install OpenLiteSpeed with WebAdmin password "123456" and email <a@cc.com>.      |
| ` ./ols1clk.sh -R 123456 -W  `       | To install OpenLiteSpeed with WordPress and MySQL root password "123456".           |
| `./ols1clk.sh --wordpressplus a.com` | To install OpenLiteSpeed with a fully configured WordPress installation at "a.com". |

## FAQs

### Why should I use ols1clk (1-Click Install) Script?

If you are trying to install WordPress on your own machine and have no idea where to start, this is your one stop shop\! This will install the web server, and optionally MySQL and WordPress as well, giving you a running start. After the script finishes, you can get straight to configuring the web server or jump right into setting up WordPress.

### Do I need super user access?

Yes, you need root access to use the script. You may use a super user account, or you may use `sudo` as well.

### Where is OLS installed to?

`/usr/local/lsws`

### Where is WordPress installed?

By default, WordPress is installed to: `/usr/local/lsws/wordpress/`

### What PHP version is installed?

The latest version from our repositories. Currently, it is version 5.6.x

### I tried installing OLS once and it threw a bunch of text at me saying there was an error\!

This one click install script should automatically install any dependencies you are missing.

### Can I use it for my existing WordPress installation?

Yes\! All you need to do is point the script to the installation path using the `--wordpresspath` option.

### Will it break my existing WordPress installation?

Open LiteSpeed cannot read `.htaccess` files. However, this script will parse your `.htaccess` file and try to extract the relevant rewrite rules. Depending on how complex the file is, there may be issues in the conversion.

### What happens when there is an OLS update?

The Web Admin says there is an update for OLS\! The script installs using your OS's package manager. Just run update in the package manager to update your installation. e.g. `yum update`
