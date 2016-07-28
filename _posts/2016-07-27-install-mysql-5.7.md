---
layout: post
title: Upgrade MySQL to 5.7
---

First, go to https://dev.mysql.com/downloads/repo/apt/ and click "Download" button, on next page, copy the link that says "No thanks, just start my download.", it should point to something like https://dev.mysql.com/get/mysql-apt-config_0.7.3-1_all.deb

Use the link to wget the file. then

```bash
    sudo dpkg -i mysql-apt-config_0.7.3-1_all.deb
```
Select 5.7 during configuration.

Remove the previous version of MySQL:

```bash
    sudo apt-get remove mysql-client-5.5 mysql-server-5.5  # or 5.6
    sudo apt-get autoremove
```

Install MySQL 5.7

```bash
    sudo apt-get update
    sudo apt-get install mysql-server-5.7
```

Upgrade databases

```bash
    sudo mysql_upgrade -u root -p --force
```
