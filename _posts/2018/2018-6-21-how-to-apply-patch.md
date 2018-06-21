---
layout: post
title: "How to apply patch"
description: "The almost perfect way to apply patch on git. Taking into account conflict handling when applying patch."
tags: [git, howto]
---

```shell
$ git am <patch-file>

## If patch cannot be applied, run below. Otherwise, ignore
$ git apply --reject <patch-file>
## Make necessary changes at this point
$ git add <unstaged-files>
$ git am --continue
$ git format-patch

## Note: Run below to abort am process
$ git abort

```
