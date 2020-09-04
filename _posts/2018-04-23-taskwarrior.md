---
layout: post
title: Install Taskwarrior server and client on Ubuntu
ghissueid: 9
---

On Ubuntu >16.04, installing Taskwarrior client is simply:

    sudo apt install task

But you need a taskwarrior server to sync from clients.  Follow these steps to set up a Taskwarrior server.

1. install dependencies:

    sudo apt install cmake uuid-dev libgmp-dev libreadline-dev

2. clone, compile, install taskwarrior:

    git clone --recurse-submodules https://github.com/GothenburgBitFactory/taskserver.git
    cd taskserver/
    cmake -DCMAKE_BUILD_TYPE=release .
    make
    sudo make install

3. configure task server:

    mkdir ~/taskd_data
    export TASKDDATA=~/taskd_data
    taskd init
    # we are still in taskserver directory from step 2
    cd pki
    vi vars

   Here change CN from "localhost" to your domain name or IP. Assume we used "your_server".

    ./generate
    cp client.cert.pem $TASKDDATA
    cp client.key.pem  $TASKDDATA
    cp server.cert.pem $TASKDDATA
    cp server.key.pem  $TASKDDATA
    cp server.crl.pem  $TASKDDATA
    cp ca.cert.pem     $TASKDDATA
    taskd config --force client.cert $TASKDDATA/client.cert.pem
    taskd config --force client.key $TASKDDATA/client.key.pem
    taskd config --force server.cert $TASKDDATA/server.cert.pem
    taskd config --force server.key $TASKDDATA/server.key.pem
    taskd config --force server.crl $TASKDDATA/server.crl.pem
    taskd config --force ca.cert $TASKDDATA/ca.cert.pem
    taskd config --force log $TASKDDATA/taskd.log
    taskd config --force pid.file $TASKDDATA/taskd.pid
    taskd config --force server your_server:53589  # change "your_server" to your real server

4. Start task server

    taskdctl start  # if it's already started, just do restart
    taskdctl status   # show if it's running

5. Create user

    # we are still in taskserver/pki directory
    taskd add org Public
    taskd add user 'Public' 'yourname'  # yourname is your username

Now that the server is installed, we need to configure our clients to use this server.
