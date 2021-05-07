---
layout: post
title: Creating AMI for AWS Batch
tags: [AWS, batch, TIL]
author: Jaewon Chung
comments : True
---

If you want to use a custom EC2 instance with necessary dependencies and appropriate settings with AWS Batch, you have to create an AMI. One of the main steps you need to complete is to have ECS agent installed, which is required for communication to instances spawned by Batch. However, before you create an AMI, you have to ensure that the ECS agent is stopped. This assumes you are using an Ubuntu, not Amazon Linux (2). 


```
sudo systemctl stop docker-container@ecs-agent.service
```

Remove the persistent data checkpoint files. By default, these files are located in the `/var/lib/ecs/data/` directory. Use the following command to remove any such files.

```
sudo rm -rf /var/lib/ecs/data/*
```

[Reference](https://docs.aws.amazon.com/batch/latest/userguide/create-batch-ami.html)
