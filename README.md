# Dockerfile for OpenPose CPU-only

## Overview

The goal of this Dockerfile is to build OpenPose with the purpose of using the `openpose/openpose.bin` example.

I couldn't find a working CPU-only variant for Docker, so I created this to enable some local testing.

I've done absolutely nothing to reduce the size of the image. As such I'm not pushing it to Dockerhub.

### What it does
* Downloads all 3 body models: BODY_25, COCO, and MPI
* Downloads hand and foot models
* Builds Caffe from source
* Disables MKL
  * When trying to compile with MKL I ran into issues. It's probably an easy fix.

## Building

Pull down the Dockerfile. Then run:

```sh
$ docker build . -f Dockerfile.cpuonly -t "seancook/openpose-cpu"
```

It takes ~8 minutes to build on my machine (2017 iMac 4.2GHz with 64MB of RAM). YMMV.

## Usage

```sh
# basic usage, runs --help
$ docker run seancook/openpose-cpu:latest
```

```sh
# if you have image file(s) in the current directory and you would like to process them
$ docker run -v`pwd`:/data -it seancook/openpose-cpu -display 0 -image_dir /data -write_images /data
```
