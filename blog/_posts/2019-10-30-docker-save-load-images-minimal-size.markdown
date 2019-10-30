---
layout: post
title:  "Docker Save and Load Images with Minimal Size"
date:   2019-10-30 22:37:41 +0300
categories: docker
---

We can save Docker images and use them on other machines. Suppose that we want to save `pytorch/pytorch:1.2-cuda10.0-cudnn7-runtime` docker image.

## Get Docker Image Information

```shell 
$ docker images --filter=reference='pytorch/pytorch:1.2*'

REPOSITORY          TAG                           IMAGE ID            CREATED             SIZE
pytorch/pytorch     1.2-cuda10.0-cudnn7-runtime   255eed982b39        6 weeks ago         3.85GB
```

Docker image size is `3.85GB`.

## Save Docker Image

We will store docker image as `pytorch_1_2.tar`

```shell
$ docker save pytorch/pytorch:1.2-cuda10.0-cudnn7-runtime > pytorch_1_2.tar
$ ll -h pytorch_1_2.tar

-rw-r--r--  1 fatih  staff   3.6G Oct 30 23:49 pytorch_1_2.tar
```

Docker image size is decreased to `3.6G`.

## Compress with gzip (Optional)

We can use gzip to compress `pytorch_1_2.tar` file.

```shell
$ gzip pytorch_1_2.tar
$ ll -h pytorch_1_2.tar.gz

-rw-r--r--  1 fatih  staff   2.0G Oct 30 23:49 pytorch_1_2.tar.gz
```

Docker image size is decreased to `2.0G` after gzip compression.


## Load Docker Image

We can load the docker image on the other machine. First, extract the gzip archive then load the image.

```shell
$ gunzip pytorch_1_2.tar.gz
$ docker load < pytorch_1_2.tar

Loaded image: pytorch/pytorch:1.2-cuda10.0-cudnn7-runtime
```

## Size Comparision

| Base Image |  .tar | .tar.gz |
|:----------:|:-----:|---------|
|   3.85GB   | 3.6GB | 2.0GB   |
