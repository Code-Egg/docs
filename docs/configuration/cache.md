---
layout: default
title: Cache
parent: Configuration
nav_order: 3
---

# Configure Server Level Cache

OpenLiteSpeed offers built-in page caching to greatly improve page load times. You can find the server-level settings in the WebAdmin Console under **Server Configuration \> Modules \> Cache** or in the configuration file located at `/usr/local/lsws/conf/httpd_config.conf`.
![](https://openlitespeed.org/wp-content/uploads/2018/07/ols-server-level-cache-settings.png)

## Cache Settings Explained

The module is pre-populated with conservative settings that you may wish to change. Here is what each setting does:

  - `enableCache`: This setting enables or disables [public cache](https://www.litespeedtech.com/support/wiki/doku.php/litespeed_wiki:cache:lscache:setup-guidline#public_cache_vs_private_cache). (Set to `1` to enable. Set to `0` to disable.) If both public and private cache are enabled, OpenLiteSpeed will save to the private cache first.
  - `qsCache`: This setting enables or disables caching of URIs with query strings. (Set to `1` to enable. Set to `0` to disable.)
  - `reqCookieCache`: This setting tells the cache how to react to requests with cookies. If enabled (set to `1`), requests with cookies will be served a response from cache if a cached copy exists. If disabled (set to `0`), requests with cookies, will not be served a response from cache (even if a cached copy of the response exists).
  - `respCookieCache`: This setting tells the cache how to treat responses with a `Set-Cookie` header. If enabled (set to `1`), responses with a `Set-Cookie` header will be cached. If disabled (set to `0`), responses with a `Set-Cookie` header will not be cached.
  - `ignoreReqCacheCtrl`: Enabling this setting (set to `1`) will tell LiteSpeed's cache to ignore any [cache control](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) settings in the request.
  - `ignoreRespCacheCtrl`: Enabling this setting (set to `1`) will tell LiteSpeed's cache to ignore any [cache control](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) settings in the response.
  - `expireInSeconds`: This setting sets the expiration time (in seconds) for cached resources.
  - `maxStaleAge`: This setting sets the maximum time (in seconds) that the cache can serve stale cache. (Stale cache refers to the serving of expired cached resources when a newer resource is not yet available.)
  - `enablePrivateCache`: This setting enables or disables [private cache](http://www.litespeedtech.com/support/wiki/doku.php?id=litespeed_wiki:litespeed:cache#public_cache_vs_private_cache). (Set to `1` to enable. Set to `0` to disable.) If both public and private cache are enabled, OpenLiteSpeed will save to the private cache first.
  - `privateExpireInSeconds`: This setting sets the expiration time (in seconds) for cached resources in a private cache.
  - `storagePath`: This setting sets the directory where cache data will be stored. Paths starting with a `/` will use an absolute path. Paths without the starting `/` will be relative to the OpenLiteSpeed root directory. The variables `$VH_ROOT`, `$VH_NAME` and `$SERVER_ROOT` can be used to designate separate caches for different virtual hosts. If this parameter is not explicitly configured, cache will be stored in a `cachedata` directory under the OpenLiteSpeed root.
  - `checkPrivateCache`: Enabling this setting (set to `1`) will tell LiteSpeed's cache to check the private cache for entries to serve from the cache.
  - `checkPublicCache`: Enabling this setting (set to `1`) will tell LiteSpeed's cache to check the public cache for entries to serve from the cache.

You can change settings from the WebAdmin Console interface, or you can `vi /usr/local/lsws/conf/httpd_config.conf` to edit the config file.
This is a sample configuration that you can copy and paste into the config file:

``` line-numbers
module cache {
  internal                1
  ls_enabled              1

storagePath $SERVER_ROOT/cachedata
checkPrivateCache   1
checkPublicCache    1
maxCacheObjSize     10000000
maxStaleAge         200
qsCache             1
reqCookieCache      1
respCookieCache     1
ignoreReqCacheCtrl  1
ignoreRespCacheCtrl 0

enableCache         0
expireInSeconds     3600
enablePrivateCache  0
privateExpireInSeconds 3600
  
}
```

## Cache Settings Inheritance

Cache settings are inherited from general to specific, and specific overrides general. So, for example, cache settings at the virtual host level are inherited from the server level. However, if the virtual host level settings are changed, they will take precedence over the server level settings. Inheritance starts with server and goes down the line through virtual host, context, and script handler.

## Set up Virtual Host-Level Cache Settings

If you run a shared hosting server, you may wish to set up vhost-level settings for each individual user's root. If no vhost-level configuration is set, the server-level configuration will be inherited.
To set up vhost-level caching, add the cache module under each virtual host, and edit the settings there in the same way that you did at the server level.
For example, go to **Example Virtual Host \> Modules \> Add**, and select `cache` from the module drop down list. Enter **Module Parameters** for cache settings, and set **Enable Module** to `Yes`.
You can change any setting that you wish, but the most important virtual host cache setting should be `storagePath $VH_ROOT/lscache`, which will set the path to be different than the server level. Each user will have the ability to purge cache from their own account.
This is a sample configuration that you may copy and paste:

``` line-numbers
module cache {
  ls_enabled              1

storagePath $VH_ROOT/lscache
checkPrivateCache   1
checkPublicCache    1
maxCacheObjSize     10000000
maxStaleAge         200
qsCache             1
reqCookieCache      1
respCookieCache     1
ignoreReqCacheCtrl  1
ignoreRespCacheCtrl 0

enableCache         0
expireInSeconds     3600
enablePrivateCache  0
privateExpireInSeconds 3600
  
}
```

## Using LSCache

Configuring the cache module at the server level is only one part of the equation. You still need to enable caching for your web apps, and this can be done by [installing the corresponding LSCache plugin](https://docs.litespeedtech.com/lscache/#available-plugins), or [using rewrite rules in `.htaccess`](https://openlitespeed.org/kb/litespeed-cache-on-openlitespeed-without-plugins/) if no plugin is available.

## Test

You can tell that LSCache is working by looking at the headers on cacheable pages. A page served from public cache will display the header `X-LiteSpeed-Cache: hit`. A page served from private cache will display the header `X-LiteSpeed-Cache: hit, private`.
![](https://openlitespeed.org/wp-content/uploads/2018/06/Priv_Cache_Header.png)
