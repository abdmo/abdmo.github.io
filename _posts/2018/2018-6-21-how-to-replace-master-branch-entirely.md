---
layout: post
title: "How to replace master branch entirely with another feature branch"
description: "Not a good practice. But desperate times call for desperate measures."
tags: [git, howto]
---

```shell
$ git checkout feature-branch
$ git merge -s ours master
$ git checkout master
$ git merge feature-branch
```
