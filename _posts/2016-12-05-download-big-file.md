---
layout: post
title: How to download a really big file
---

I tried to download a really big file.  It is an rar file, sized 2.5G, the uncompressed file is supposely 8G.  Chorme on Windows fully downloaded file, but when I tried to uncompress it, I got "file is corrupted" error.

So I turn to Linux, using wget, it also fully downloaded the file, during dowloading, there are `reconnects`:

```bash
    wget http://somesite.com/download/bigassfile.rar
    ...
    2016-12-06 00:37:03 (6.53 MB/s) - Connection closed at byte 1076146848. Retrying.
```

When I tried to uncompress it, it gave:

```bash
    unrar e bigassfile.rar

    UNRAR 5.00 beta 8 freeware      Copyright (c) 1993-2013 Alexander Roshal

    Extracting from bigassfile.rar

    Extracting  bigassfile.txt                                          76%
    bigassfile.txt     - checksum error
    Total errors: 1
```

Same problem as on Windows.

I downloaded a few more times, every time I got error, every time md5sum of the file are different.

I suspect the problem was caused by reconnect, maybe the webserver hosting the file has a timeout setting. How can I get a consistent connection during the process of downloading?  I thought of curl`s range option, it can download part of file in the specified range.  I can download one fifth of the file 5 times then concatenate them into a single file, because each download is much smaller, it will not timeout.  If it timeouts again, I just need to break them into even smaller parts.

```bash
    curl -r 0-500000000  http://somesite.com/download/bigassfile.rar -o 1.rar
    curl -r -500000001-1000000000 http://somesite.com/download/bigassfile.rar -o 2.rar
    curl -r 1000000001-1500000000 http://somesite.com/download/bigassfile.rar -o 3.rar
    curl -r 1500000001-2000000000 http://somesite.com/download/bigassfile.rar -o 4.rar
    curl -r 2000000001- http://somesite.com/download/bigassfile.rar -o 5.rar
```

all 5 parts are downloaded without timeout. Let`s combine them into one.

```bash
    cat 1.rar 2.rar 3.rar 4.rar 5.rar > bigassfile.rar
```

Now the moment of truth:

```bash
    unrar e bigassfile.rar

    UNRAR 5.00 beta 8 freeware      Copyright (c) 1993-2013 Alexander Roshal


    Extracting from bigassfile.rar

    Extracting  bigassfile.txt                                          OK
    All OK
```

It worked.
