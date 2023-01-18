---
layout: post
title: Fix WSL not connecting to internet
tags: [WSL, TIL]
author: Jaewon Chung
comments: True
---

WSL sometimes cannot connect to the internet and the following message appears when you try to use `apt`, `git push/pull`, etc:

```{bash}
Temporary failure in name resolution
```

The reason this error occurs is because Windows automatically generates `resolv.conf` file with wrong nameserver.

To resolve the issue, edit the `resolv.conf` file as follows:

1. Remove the symlink to `/run/resolvconf/resolv.conf`

```
sudo rm /etc/resolv.conf
```

1. Create and open new `/etc/resolv.conf` file

```{bash}
sudo nano /etc/resolv.conf
```

2. Copy and paste the following contents

```{bash}
[network]
generateResolvConf = false
nameserver 8.8.8.8
```

4. Note that you can use any of the free nameserver (DNS server) such as `1.1.1.1` (Cloudflare) or `8.8.8.8` (Google)

5. Save and exit the file.

WSl should be able to connect to the internet at this point!
