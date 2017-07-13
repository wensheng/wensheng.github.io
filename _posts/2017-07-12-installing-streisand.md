---
layout: post
title: Installing Streisand on Linode
ghissueid: 6
---

[Streisand](https://github.com/jlund/streisand) is a set of [Ansible](https://github.com/ansible/ansible) playbooks that set up a VPN automatically.

It can create a new virtual machine on one of the cloud providers, such as Linode, from a remote server.  Since I already have a spare Linode, I decided to set up Streisand on that server `locally`.  Streisand run only on Ubuntu 16.04, my linode is 16.04 so it's all good.

Streisand installation requires AppArmor enabled in the kernel.  The custom kernel by Linode does not have apparmor.  I have to edit the linode profile and change the kernel to `GRUB 2` (so it will pick the kernel that comes with the distro), reboot and verify apparmor is enabled by `sudo apparmor_status`.

Then install the dependencies:

    sudo apt install libssl-dev
    sudo pip install ansible markupsafe linode-python

Get Streisand:

    git clone https://github.com/jlund/streisand.git 
    cd streisand

Install Streisand:

    sudo su
    ./streisand

Select `localhost (Advanced)` as provider, then wait for the setup to complete, it should take about 10-15 minutes.

If the installation is successful, a folder called `generated-docs` will be created, inside it are the directions (in html format) for setting up clients.

First download and install the SSL certificate by following the instruction in the html file. Then following the instructions in `Connecting to your Streisand Gateway` section at the bottom of that page.

Open the website from that page, enter the usename and password, we are presented with `Connection Instructions`.  There are several clients to choose from.  I chose OpenVPN(direct) since it's simple.

Download OpenVPN windows installer, install and run OpenVPN.  Download of the OpenVPN profiles from the web page, I just chose the first: `client-1`.  Right click OpenVPN taskbar icon and select `Import file...` and import the profile that's just downloaded.  Then right click again and select `Connect`.  Now we should be connected to our VPN.  Go to [whatismyip.com](https://www.whatismyip.com/) to verify, the IP address it detected should be the IP of the streisand server.  Disconnect and the IP detected will be back to the real IP.
