---
layout: post
title: Move files from all sub-directories to current directory
ghissueid: 2
---

On Linux or Windows (with Windows Subsystem for Linux, or [Bash on Ubuntun on Windows](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) ), if you want to move all files from all sub-directories to the current one, do this:

```
find ./*/ -type f -exec mv -t . '{}' +
```

You can then remove all those empty directories:

```
find ./* -type d -exec rmdir '{}' +
```
