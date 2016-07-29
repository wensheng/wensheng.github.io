---
layout: post
title: Installing Pypy3
---

Go to http://pypy.org/download.html and find the download link and wget/extract it.

```bash
    wget https://bitbucket.org/pypy/pypy/downloads/pypy3.3-v5.2.0-alpha1-linux64.tar.bz2
    tar xfj pypy3.3-v5.2.0-alpha1-linux64.tar.bz2 
```

Preferably move it to /opt/ or /usr/local/:

```bash
    sudo mv pypy3.3-v5.2.0-alpha1-linux64 /opt/
```

Install pip and virtualenv

```bash
    wget https://bootstrap.pypa.io/get-pip.py
    sudo /opt/pypy3.3-v5.2.0-alpha1-linux64/bin/pypy3 get-pip.py 
    sudo /opt/pypy3.3-v5.2.0-alpha1-linux64/bin/pypy3 -m pip install virtualenv
```

Use `/opt/pypy3.3-v5.2.0-alpha1-linux64/bin/virtualenv` for everything afterward.
