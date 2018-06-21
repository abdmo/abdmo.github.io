---
layout: post
title: "How to rebase"
description: "How to do git rebase in different cases."
tags: [git, howto]
---

#### Rebase to a specific commit (Not really sure about this. TODO: verify this):
```shell
$ git rebase -i HEAD ~ <index-of-the-commit-from-HEAD>

## Choose 'edit' for the commit that will be changed
## Do changes

## Continue rebasing
git rebase --continue

```

#### Rebase feature branch with master:
```shell
## Run below on feature-branch
## Backup feature branch
$ git checkout -b feature-branch-backup
## Update all tracking branches
$ git fetch origin
## Rebase on master
$ git rebase origin/master
```