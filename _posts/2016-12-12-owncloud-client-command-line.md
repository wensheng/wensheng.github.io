---
layout: post
title: Use ownCloud linux command line client
---

I use Linux, but rarely its desktop.  I mostly just use command line. OwnCloud provides a command line program: owncloudcmd. Here are the steps to set it up and use it.

First install the OwnCloud client.  Go to: 
https://software.opensuse.org/download/package?project=isv:ownCloud:desktop&package=owncloud-client
 and select Linxu flavor and version.  I use Ubuntu 16.04:

```bash
    sudo sh -c "echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/desktop/Ubuntu_16.04/ /' > /etc/apt/sources.list.d/owncloud-client.list"
    sudo apt-get update
    sudo apt-get install owncloud-client
```

To do an owncloud sync, you need to know its webdav url. It`s usually:

`http://serveraddress/remote.php/webdav/folder_path`

If you unsure of the URL. just login to your owncloud web interface, click on `Settings` at the lower left corner, it will show `WebDAV`, copy the address, it should end with `/remote.php/webdav/`.

To sync a specific folder or file, append the path after `/webdav/`.  For example I have top level `wiki` folder I want to sync, its url is: `http://myserver/remote.php/webdav/wiki`, on the command line:

```bash
    owncloudcmd -n ownCloud/wiki  http://myserver/remote.php/webdav/wiki
```

It will prompt for username and password.  The program exits when it finish syncing.
