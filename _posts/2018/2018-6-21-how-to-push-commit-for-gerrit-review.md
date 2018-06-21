---
layout: post
title: "How to push commit for gerrit review"
description: "Pushing commit to gerrit review."
tags: [gerrit, git, howto]
---

```shell
## Push to specific branch
$ git push origin <sha>:refs/changes/<branch>

## Push as a new version of an existing commit on gerrit
$ git push origin <sha>:refs/changes/<change-id>

```
