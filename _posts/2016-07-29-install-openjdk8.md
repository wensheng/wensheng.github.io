---
layout: post
title: Installing OpenJDK8 or Oracle jdk8
tags: java jdk linux
---

Needed to do a project in Java8, here's how to install openjdk8:

```bash
    sudo add-apt-repository ppa:openjdk-r/ppa
    sudo apt-get update
    sudo apt-get install openjdk-8-jdk
    sudo update-alternatives --config java  # select openjdk8
    sudo update-alternatives --config javac  # select openjdk8
    sudo apt-get purge maven maven2 maven3
    sudo add-apt-repository ppa:andrei-pozolotin/maven3
    sudo apt-get update
    sudo apt-get install maven3
```

If want Oracle Java 8 JDK:

```bash
    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java8-installer
    sudo vi /etc/environment 
    # add JAVA_HOME="/usr/lib/jvm/java-8-oracle"
```
