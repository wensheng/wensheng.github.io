---
layout: post
title: webpack babel error
---

If you see something like this when you do `webpack`:

```bash
ERROR in ./app/app.js
Module build failed: Error: Couldn't find preset "es2015" relative to directory "/home/yourname"
```

That's becuase you have a `.babelrc` in your home directory but not in your project directory.  To get rid of this error, either deleting the `.babelrc` in your home directory or creating one in your current project diretory.
