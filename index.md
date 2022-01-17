---
layout: default
title: Home
nav_order: 1
description: "OpenLiteSpeed is a high-performance, lightweight, open source HTTP server."
permalink: /
---

# High Performence Web Server
{: .fs-9 }

OpenLiteSpeed is a high-performance, lightweight, open source Web server, provide user multiple easy setup method.
{: .fs-6 .fw-300 }

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [View it on GitHub](https://github.com/litespeedtech/openlitespeed){: .btn .fs-5 .mb-4 .mb-md-0 }

---

OpenLiteSpeed is the Open Source edition of LiteSpeed Web Server Enterprise. Both servers are actively developed and maintained by the same team, and are held to the same high-quality coding standard. OpenLiteSpeed contains all of the essential features found in LiteSpeed Enterprise, and represents our commitment to support the Open Source community.

## Getting started

- [Installation](/docs/installation/repo)
- [Configuration](/docs/configuration)
- [PHP](/docs/php)

### Supported OS 

- [Debian 7, 8, 9, 10](https://www.debian.org/distrib/)
- [Ubuntu 16, 18, 20](https://www.ubuntu.com/download)
- [CentOS 6, 7, 8](https://www.centos.org/download/)
- [AlamaLinux 8](https://mirrors.almalinux.org/isos.html)



## About the project

OpenLiteSpeed is &copy; 2013-{{ "now" | date: "%Y" }} by [LiteSpeedtech](https://www.litespeedtech.com/).

### License

OpenLiteSpeed is distributed by an [GPLv3 license](https://www.litespeedtech.com/open-source/openlitespeed).

### Contributing
Thank you to the contributors of OpenLiteSpeed!

<ul class="list-style-none">
{% for contributor in site.github.contributors %}
  <li class="d-inline-block mr-1">
     <a href="{{ contributor.html_url }}"><img src="{{ contributor.avatar_url }}" width="32" height="32" alt="{{ contributor.login }}"/></a>
  </li>
{% endfor %}
</ul>


