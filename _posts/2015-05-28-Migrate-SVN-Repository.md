---
layout: default
title: Migrate SVN Repository
comments: true
---

## Step 1: Backup your old Repository

Dump your old repository with command:

```shell
svnadmin dump /path/to/repository > repo.dump
```

## Step 2: Import your old repository into the new one

The new repository will load your dumpped data 

```shell
svnadmin load /path/to/repository < repo_name.svn_dump
```
