---
layout: post
title: "Print variable in Makefile"
description: "Neat stuff"
tags: [c, howto]
---

Add a new rule to the Makefile:
```c
print-%: ; @echo $* = $($*)
```

To use it:
```shell
$ make print-<VARIABLE>
```