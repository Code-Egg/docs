---
layout: default
title: LiteSpeed Repository
parent: Installation
nav_order: 2
---

# Install OLS from LiteSpeed Repositories

## AlmaLinux and CentOS Installation

Follow these steps for AlmaLinux 8 or CentOS 6, 7, 8 systems.

### Add the Repository

Use the following commands to add our CentOS repositories:
AlmaLinux 8 & CentOS 8: 

    rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el8.noarch.rpm

CentOS 7:

    rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el7.noarch.rpm

CentOS 6: 

    rpm -Uvh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el6.noarch.rpm

### Install OpenLiteSpeed

The following command installs the latest version of OpenLiteSpeed.

``` line-numbers
yum install openlitespeed
```

You can also specify version numbers. For example `yum install openlitespeed-1.6.20`, will install OLS v1.6.20.
**Note:** The OpenLiteSpeed packages in our repositories have SPDY enabled. The binary also includes the OpenSSL library needed to run SPDY. You do not have to install OpenSSL 1.0.1 to use SPDY if you download the package from the repositories.

### Install PHP

The following commands will install PHP 7.3 for OpenLiteSpeed from our repository with all commonly-used packages, and direct OpenLiteSpeed to use this PHP. This build of PHP should be enough to support the most commonly used web applications.

``` line-numbers
yum install epel-release
yum install lsphp73 lsphp73-common lsphp73-mysql lsphp73-gd lsphp73-process lsphp73-mbstring lsphp73-xml lsphp73-mcrypt lsphp73-pdo lsphp73-imap lsphp73-soap lsphp73-bcmath
ln -sf /usr/local/lsws/lsphp73/bin/lsphp /usr/local/lsws/fcgi-bin/lsphp5
```

If you wish to install another version such as PHP 8.0, replace `lsphp73` with that other version, for example `lsphp80`.
To use a custom PHP build, see our wikis on PHP via RPM and building a custom PHP from [the source](https://docs.litespeedtech.com/extapp/php/getting_started/#source).
If you prefer not to use symbolic link setup for PHP, please log into the WebAdmin Console at port 7080, and update the PHP version/path via the **Server Configuration \> External App \> Command**.

## Debian and Ubuntu Installation

Folow these steps for Debian 7, 8, 9, 10 or Ubuntu 14.04, 16.04, 18.04, 20.04 systems.

### Add the Repository

Use the following command to add our Debian repository:
Debian 7, 8, 9, 10 & Ubuntu 14,16,18, 20 :

    sudo wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | sudo bash

### Install OpenLiteSpeed

The following command installs the latest version of OpenLiteSpeed.

``` line-numbers
apt-get install openlitespeed
```

You can also specify version numbers. For example `apt-get install openlitespeed-1.6.20`, will install OLS v1.6.20.
**Note:** The OpenLiteSpeed packages in our repositories have SPDY enabled. The binary also includes the OpenSSL library needed to run SPDY. You do not have to install OpenSSL 1.0.1 to use SPDY if you download the package from the repositories.

### <span class="mw-headline">Install PHP</span>

The following commands will install PHP 7.3 with all commonly-used packages for OpenLiteSpeed from our Debian Repo, and direct OpenLiteSpeed to use this PHP. This build of PHP should be enough to support the most commonly used web applications.

``` line-numbers
apt-get install lsphp73 lsphp73-common lsphp73-curl lsphp73-mysql lsphp73-opcache lsphp73-imap lsphp73-opcache 
ln -sf /usr/local/lsws/lsphp73/bin/lsphp /usr/local/lsws/fcgi-bin/lsphp5
```

If you wish to install another version such as PHP 8.0, replace `lsphp73` with that other version, for example `lsphp80`.
To use a custom PHP build, see our wikis on PHP via RPM and building a custom PHP from [the source](https://docs.litespeedtech.com/extapp/php/getting_started/#source).
If you prefer not to use symbolic link setup for PHP, please log into the WebAdmin Console at port 7080, and update the PHP version/path via the **Server Configuration \> External App \> Command**.

## Getting Started

OpenLiteSpeed's default installation directory is `/usr/local/lsws`. For detailed information on controlling the server processes, please see the [Administration Guide](https://openlitespeed.org/kb/command-references-for-administration/).

### Start the Server

To start the server, run `systemctl start lsws`. (If you ever want to stop the server, you can run `systemctl stop lsws`.)
A sample site should now be running on the server.
To access your site, point your browser to `http://[address]:8088/`, `[address]` being the IP address or domain name of your web server machine. Use `localhost` if the server is on the same machine that you are currently using. By default, OpenLiteSpeed runs on port `8088`.
A Congratulations page linked to other testing pages should load into the browser when pointed to the above address.

### Troubleshooting

If the Congratulations page does not appear, try testing the WebAdmin interface, like so:

  - Plug `https://[address]:7080/`, into your browser to access the WebAdmin Console (The default port for the WebAdmin Console is `7080`).
  - Remember the `https://` and that for `[address]` you can use `localhost` if you're currently using the machine the server is on.
  - A login page should load. The defaults for the administrator's user name and password are `admin` and a randomly generated password.
  - For detailed information regarding configuration, please refer to our [Configuration Guide](https://openlitespeed.org/kb/category/configuration/), or click the **Help** link at the top of each page.

Some other ideas:

  - If your server uses a firewall, please make sure that `localhost` is trusted. For instance, Linux with IPTables should include a rule `ALLOW INPUT from LO`.
  - Take a look at the error log found at `/usr/local/lsws/logs/error.log` for a possible explanation.
  - If there is a TCP port conflict with other server applications, you will need to stop the application currently running on port 7080. The following command can be used to check port 7080: `netstat -an | grep 7080`. If the port is available, the command will produce no output.
  - If the swapping directory is not writable, you can either grant writing permission for the swapping directory to the user whom the web server is running as, or change the swapping directory's configuration manually. The swapping directory is configured in the server's XML configuration file found at `/usr/local/lsws/conf/httpd_config.xml`. Search the XML file for "swappingDir". The default location for the swapping directory is `/tmp/lshttpd/swap`.

If none of these ideas help, and you still have a problem with installation, please visit the [OpenLiteSpeed Forum](https://forum.openlitespeed.org/) for assistance.

## Video Tutorial
<iframe title="YouTube video player" src="https://www.youtube.com/embed/urnHwEQ2eAE" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe>
