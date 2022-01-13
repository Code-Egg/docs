---
layout: default
title: Configuration
parent: PHP
nav_order: 3
---

# Set up OLS with Multi PHP versions

## Multiple PHP Versions on OpenLiteSpeed

Users may want to setup multi-PHP versions and use a different version for different virtual host/domain.
This wiki will address setting up multiple PHPs for one installation of OpenLiteSpeed. The wiki assumes you have [our repository](https://openlitespeed.org/mediawiki/index.php/Help:PHP_via_RPM "Help:PHP via RPM") installed and enabled and that PHP is being downloaded from this repository. Multiple PHPs can just as easily be set up when PHP has been built from source, though. This wiki demonstrates installing different versions of PHP. Alternately, you can install the same version of PHP with different options. This wiki uses the WebAdmin console where possible.
Multi-PHP versions will be configurated to different external apps, while the script handler section will define which version to be used.

## <span id="Install_multiple_PHPs" class="mw-headline">Install Multiple PHPs</span>

``` line-numbers
yum groupinstall lsphp-all
```

The above command installs all available versions of PHP (5.3, 5.4, 5.5, 5.6, 7.0, 7.1, 7.2, 7.2, 7.3).
These PHP builds are installed to the following locations based on version number:

``` line-numbers
/usr/local/lsws/lsphp53/bin/lsphp
/usr/local/lsws/lsphp54/bin/lsphp
/usr/local/lsws/lsphp55/bin/lsphp
/usr/local/lsws/lsphp56/bin/lsphp
/usr/local/lsws/lsphp70/bin/lsphp
/usr/local/lsws/lsphp71/bin/lsphp
/usr/local/lsws/lsphp72/bin/lsphp
/usr/local/lsws/lsphp73/bin/lsphp
```

## <span id="Define_external_applications_for_these_PHPs" class="mw-headline">Define External Applications for These PHPs</span>

We will go through creating an external application for PHP 5.6. However, you can also use the default external applications with some modification.
Add an external application (in the WebAdmin console \> Server Configuration \> External App \> Add).
![](https://openlitespeed.org/wp-content/uploads/2018/06/ols-install-lsphp56-570x276.png)
Choose LSAPI for the application type.
![](https://openlitespeed.org/wp-content/uploads/2018/06/ols-install-lsphp56-2-570x276.png)
Give the new external application an appropriate name, address, maximum number of connections, initial response timeout, and retry timeout.
![](https://openlitespeed.org/wp-content/uploads/2018/06/ols-install-lsphp56-3-570x276.png)
 
The most important setting is the Command setting. This tells the external application where to look for the PHP it will use. Direct it to one of your PHP installations.

## <span id="Set_up_script_handlers" class="mw-headline">Set up Script Handlers</span>

Script handlers will tell OpenLiteSpeed which scripts should go to which external application. There are many ways to configure this. You can, for example, stipulate that different external applications should be responsible for different suffixes. We will be setting up a script handler at the virtual host level to tell OpenLiteSpeed that our new lsphp54 external application will be serving `.php` scripts in this virtual host. As a result, this will override any server-level script handler settings. Use context-level settings to override server- or virtual host-level settings.
Add a virtual host-level script handler (WebAdmin console \> Server Configuration \> Virtual Hosts \> Example \> Script Handlers \> Add).
![](https://openlitespeed.org/wp-content/uploads/2018/06/add-script-handler-570x276.png)
 
 
 
Choose the suffix this script handler will handle, make the Handler Type `LiteSpeed SAPI`, and direct it toward the external application you just created.
![](https://openlitespeed.org/wp-content/uploads/2018/06/add-script-handler2-570x276.png)
Graceful restart to save your changes.
 
![](https://openlitespeed.org/wp-content/uploads/2018/06/ols-install-lsphp56-7-570x276.png)
OpenLiteSpeed will now serve `.php` scripts in the virtual host with PHP 5.6. You can repeat these processes for any other PHP builds you'd like to support.
