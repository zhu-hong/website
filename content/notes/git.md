---
title: "git常用命令"
date: 2021-05-17T09:29:47+08:00
draft: true
tags: [git]
---

```
git init
git add .
git commit -m 'description'

git remote add Alias Url
git remote rm Alias

git pull --rebase origin main

git checkout -b gh-pages
git add -f dist
git subtree push --prefix dist origin gh-pages
```