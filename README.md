# Dockerfile for OpenPose CPU-only

## Overview

The goal of this Dockerfile is to build [OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose) with the purpose of using the `openpose/openpose.bin` example. I couldn't find a working CPU-only variant for Docker, so I created this to enable local testing.

### What it does

* Downloads all 3 body models: `BODY_25`, `COCO`, and `MPI`
* Downloads `hand` and `foot` models
* Builds Caffe from source
* Disables MKL
  * When trying to compile with MKL I ran into issues. It's probably an easy fix.

## Building

****
Pull down the Dockerfile. Then run:

```sh
$ docker build . -f Dockerfile.cpuonly -t "seancook/openpose-cpu"
```

## Troubleshooting

* If you experience issues while running the the `make` command, please lower the number of jobs to something that makes sense for your machine. See [discussion here](https://github.com/seancook/docker-openpose-cpu/pull/2). Thanks to `jonathanmv` for pointing this out.
* If you change the commit hash to use (line 28 of the Dockerfile), be aware that dependencies have likely changed so you should update the dependencies installed at the start of the file.

## Usage

****

### A note about memory

If you run the container and see the message 'Killed', or it silently fails, you need to update the available memory for docker containers. [See here for more information](https://stackoverflow.com/questions/44533319/how-to-assign-more-memory-to-docker-container).

To test the container:

```sh
# basic usage, runs --help
$ docker run seancook/openpose-cpu:latest
```

To perform inference on a directory of images:

```sh
# performs inference using example images and saves them to working directory
$ docker run -v`pwd`:/data -it seancook/openpose-cpu -display 0 -image_dir /data -write_images /data
```
