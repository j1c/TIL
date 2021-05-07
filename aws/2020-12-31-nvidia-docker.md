---
layout: post
title: Setting nvidia as default runtime 
tags: [AWS, EC2, TIL]
author: Jaewon Chung
comments : True
---

When running GPU EC2 instances, sometimes it is useful to run all docker containers with GPUs. Typically, the command looks like this:

```
docker run -ti --gpus 1 <container/image>
```

where the `--gpus` option specify the GPU you want available for your container. If you have more than one GPU and want all of them to be available in the container, you could pass `--gpus all` instead. The `--gpus` option will automatically set the docker runtime to `nvidia`.

Modify the `/etc/docker/daemon/json` as follows:

```
{
  "default-runtime": "nvidia",
  "runtimes": {
    "nvidia": {
      "path": "/usr/bin/nvidia-container-runtime",
      "runtimeArgs": []
    }
  }
}
```

then run

```
sudo systemctl restart docker
```

Confirm that the `nvidia` is the default runtime by running:
```
docker info | grep -i runtime
```

You should see something like 
```
 Runtimes: nvidia runc
 Default Runtime: nvidia
```

If you have a fresh machine, you will most likely need to install the NVIDIA Container Runtime. See [here](https://docs.nvidia.com/dgx/nvidia-container-runtime-upgrade/index.html#about-nv-container-runtime) for instructions.
