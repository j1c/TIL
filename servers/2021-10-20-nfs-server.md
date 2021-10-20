---
layout: post
title: Setting nvidia as default runtime
tags: [Ubuntu, TIL]
author: Jaewon Chung
comments: True
---

```
sudo exportfs -rv
```

```
-r

Reexport all directories, synchronizing /var/lib/nfs/etab with /etc/exports. This option removes entries in /var/lib/nfs/etab which have been deleted from /etc/exports, and removes any entries from the kernel export table which are no longer valid.

-v

Be verbose. When exporting or unexporting, show what's going on. When displaying the current export list, also display the list of export options.
```

then run

```
sudo mount -a
```
