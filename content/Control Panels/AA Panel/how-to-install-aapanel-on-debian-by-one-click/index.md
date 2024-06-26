---
title: "How to install aaPanel on Debian by one click"
date: "2022-12-05"
title_meta: "GUIDE How to install aaPanel on Debian"
description: "AaPanel offers a control panel for managing web hosting environments on Linux. While aaPanel advertises a one-click installation, it typically involves downloading a script and executing it with root privileges. This guide outlines the steps for installing aaPanel on Debian."
keywords:  [ 'Debian', 'aaPanel', 'installation']
tags: ["aaPanel"]
icon: "Debian"
date: "2024-03-07T17:25:05+01:00"
lastmod: "2024-03-07T17:25:05+01:00" 
draft: false
toc: true
aliases: ['/Linux/Debian/how-to-install-aapanel-on-debian-by-one-click/']
tab: true
---

![How to install aaPanel on Debian by one click](images/How-to-install-aaPanel-on-Ubuntu-by-one-click-2-1024x576.png)

## Introduction

In this article, you will learn how to install [aaPanel](https://utho.com/docs/tutorial/how-to-migrate-accounts-from-cwp-to-cwp/) on Debian by one click.

aaPanel is a control panel for web servers that is an alternative to cPanel and Vesta. It was developed in China. BT.cn is responsible for its development, and it is now on version v6. 8.5 (at the moment of the writing). It is free, has reached an adequate level of maturity, and comes with some really excellent features such as an editor, uploader, file management, backups, and preconfigured [Nginx](https://www.nginx.com/) rules.

Note: If you are setting up a new system that has not yet had other environments such as Apache/Nginx/php/MySQL installed, you will need to install aaPanel.

The following components are often included in web hosting control panels:

- [Web server](https://en.wikipedia.org/wiki/Web_server) (e.g. [Apache HTTP Server](https://en.wikipedia.org/wiki/Apache_HTTP_Server), [NGINX](https://en.wikipedia.org/wiki/Nginx), [Internet Information Services](https://en.wikipedia.org/wiki/Internet_Information_Services))
- [Domain Name System](https://en.wikipedia.org/wiki/Domain_Name_System) server
- [Mail server](https://en.wikipedia.org/wiki/Mail_server) and [spam](https://en.wikipedia.org/wiki/Messaging_spam) filter
- [File Transfer Protocol](https://en.wikipedia.org/wiki/File_Transfer_Protocol) server
- [Database](https://en.wikipedia.org/wiki/Database)
- [File manager](https://en.wikipedia.org/wiki/File_manager)
- [System monitor](https://en.wikipedia.org/wiki/System_monitor)
- [Web log analysis software](https://en.wikipedia.org/wiki/Web_log_analysis_software)
- [Firewall](https://en.wikipedia.org/wiki/Firewall_(computing))
- [phpMyAdmin](https://en.wikipedia.org/wiki/PhpMyAdmin)

## Installation

```
# wget -O install.sh http://www.aapanel.com/script/install-ubuntu_6.0_en.sh && bash install.sh 93684c35
```

![output](images/image-570.png)

Paste the aapanel Internet address in your browser to login into the aapanel, which is shown in the previous screenshot. Username and password are also shown in the screenshot.

![install aaPanel on Debian](images/image-569.png)

## Conclusion

Hopefully, you have learned how to install aaPanel on Debian by one click.

Thank You 🙂
