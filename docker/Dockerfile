ARG TENSORRT_VERSION=19.06-py3
FROM nvcr.io/nvidia/tensorrt:${TENSORRT_VERSION}

LABEL maintainer=binwei.yang@gmail.com

# Install package dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        libprotobuf-dev \
        protobuf-compiler \
        swig \
    && rm -rf /var/lib/apt/lists/*

# Install onnx
RUN pip install onnx

# Build the library
ENV ONNX2TRT_VERSION 0.1.0
ARG CUDA_VERSION=10.1

WORKDIR /opt
RUN git clone --recurse-submodules https://github.com/onnx/onnx-tensorrt.git && \
    cd onnx-tensorrt && \
    mkdir build && \
    cd build && \
    # see https://github.com/onnx/onnx-tensorrt/issues/93
    cmake .. -DTENSORRT_ROOT=/opt/tensorrt -DCUDA_INCLUDE_DIRS=/usr/local/cuda-${CUDA_VERSION}/include && \
    make -j$(nproc) && \
    make install && \
    ldconfig && \
    cd .. && \
    python setup.py build && \
    python setup.py install && \
    rm -rf ./build/

RUN ["/bin/bash"]
