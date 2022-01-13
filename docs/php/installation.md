---
layout: default
title: Installation
parent: PHP
nav_order: 2
---

#### LiteSpeed Repository

The easiest way to get up and running with PHP is to use the LiteSpeed Repository. The LiteSpeed Repository comes with prebuilt PHP packages with LiteSpeed support built in.

Here is a list of some of the features you get when you use our LiteSpeed Repository, instead of compiling PHP manually:

* PHP versions 5.3^\*^, 5.4^\*^, 5.5^\*^, 5.6^\*^, 7.0^\*^, 7.1, 7.2, 7.3 and 7.4.
* CentOS and RHEL versions 6, 7, and 8.
* Ubuntu versions 14.04, 16.04, 18.04 and 20.04.
* Debian versions 8, 9, and 10.
* Contains most up-to-date versions of LSAPI. (Do not have to wait for a new PHP version to be released.)
* Easily install multiple versions of PHP (by default installed to different locations).
* Contains all frequently used PHP packages.
* Contains multiple possible MySQL support packages (via native driver or client library).
* Contains multiple opcode caching options: APCu, Xcache, Zend Opcache.

!!! example "Footnote"
    **\*:** These PHP versions are dependent on the OS. Newer OS's might not support the older end-of-life (EOL) versions.

##### Prerequisites {: #litespeed-repo-prereqs }

Before you can install LSPHP there are a couple of prerequisite packages and repositories that need to be installed via the operating system's package manager. This can be accomplished by running the following commands in a terminal:

=== "CentOS 6"

    ``` bash
    $ sudo yum install http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el6.noarch.rpm
    $ sudo yum install https://rpms.remirepo.net/enterprise/remi-release-6.rpm
    $ sudo yum install epel-release
    ```

=== "CentOS 7"

    ``` bash
    $ sudo yum install http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el7.noarch.rpm
    $ sudo yum install epel-release
    ```

=== "CentOS 8"

    ``` bash
    $ sudo dnf install http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el8.noarch.rpm
    $ sudo dnf install epel-release
    ```

=== "Ubuntu/Debian"

    ``` bash
    $ sudo wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | sudo bash
    ```

##### Packages {: #litespeed-repo-search-packages }

It is not a requirement to search through the LSPHP packages and extensions we offer before installing, but it does come in handy to know what you can install and then have the option to install everything at the same time. To find out all the LSPHP packages and extensions we offer you can run the following:

=== "CentOS 6/7"

    ``` bash
    $ sudo yum search lsphp
    ```

=== "CentOS 8"

    ``` bash
    $ sudo dnf search lsphp
    ```

=== "Ubuntu/Debian"

    ``` bash
    $ sudo apt-cache search lsphp
    ```

##### Install {: #litespeed-repo-install-lsphp }

Now that you have installed the prerequisites you can go ahead with the actual installataion process.

If you searched our repository then you can use the names of the packages and extensions you found to install what you need. 

For example, to install LSPHP74 and LSPHP74-mysql via the command line you would run the following:

=== "CentOS 6/7"

    ``` bash
    $ sudo yum install lsphp74 lsphp74-common lsphp74-mysqlnd
    ```

=== "CentOS 8"

    ``` bash
    $ sudo dnf install lsphp74 lsphp74-common lsphp74-mysqlnd
    ```

=== "Ubuntu/Debian"

    ``` bash
    $ sudo apt-get install lsphp74 lsphp74-common lsphp74-mysql
    ```

This will install `lsphp74` and `lsphp74-mysql` into the following location: `/usr/local/lsws/lsphp74/bin/lsphp`.

Once it has been installed you can continue to the [setup section](#setup).