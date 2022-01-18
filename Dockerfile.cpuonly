FROM ubuntu:18.04

LABEL maintainer="sean@seancook.dev"
LABEL description="CPU-only version of OpenPose. Not slimmed for production."
LABEL version="1.1"

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y \
        wget apt-utils lsb-core cmake git \
        libopencv-dev \
        protobuf-compiler \
        libprotobuf-dev \
        libgoogle-glog-dev \
        libboost-all-dev \
        hdf5-tools \
        libhdf5-dev \
        libatlas-base-dev

RUN git clone https://github.com/CMU-Perceptual-Computing-Lab/openpose.git

WORKDIR openpose

# Latest commit as of 18 Jan 22.
# Change if you need to, but be aware that dependencies may have changed.
RUN git checkout 6f0b8868bc4833b4a6156f020dd6d486dcf8a976

WORKDIR scripts/ubuntu

RUN sed -i 's/\<sudo -H\>//g' install_deps.sh; \
    sed -i 's/\<sudo\>//g' install_deps.sh; \
    sync; sleep 1;

WORKDIR /openpose/build

# Downloads all available models. You can reduce image size by being more selective.
RUN cmake -DGPU_MODE:String=CPU_ONLY \
          -DOWNLOAD_BODY_25_MODEL:Bool=ON \
          -DDOWNLOAD_BODY_MPI_MODEL:Bool=ON \
          -DDOWNLOAD_BODY_COCO_MODEL:Bool=ON \
          -DDOWNLOAD_FACE_MODEL:Bool=ON \
          -DDOWNLOAD_HAND_MODEL:Bool=ON \
          -DUSE_MKL:Bool=OFF \
          ..

# you may find that you need to adjust this.
RUN make -j$((`nproc`+1))

RUN apt-get remove wget unzip cmake git build-essential -y && apt-get autoremove -y

WORKDIR /openpose

ENTRYPOINT ["build/examples/openpose/openpose.bin"]

CMD ["--help"]
