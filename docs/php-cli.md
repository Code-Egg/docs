---
layout: default
title: PHP setup with CLI
parent: PHP
permalink: /docs/php/cli
---

## Setup PHP at server level
### Define External Applications for PHPs

Edit httpd_config.conf, 
```
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

### Set up Script Handlers 

Edit httpd_config.conf, 
```
scriptHandler{
    add lsapi:lsphp80  php
}
```
