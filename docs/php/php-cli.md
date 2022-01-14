---
layout: default
title: PHP Setup With CLI
nav-order: 2
parent: PHP
permalink: /docs/php/cli
---

# Set Up LSPHP at the Server Level From the Command Line
{: .no_toc}

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

## Define External Applications for PHPs

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

## Set up Script Handlers 

Add the following to the `httpd_config.conf` file:

```bash
scriptHandler{
    add lsapi:lsphp80  php
}
```
