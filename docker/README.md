# onnx2trt

## Build docker container
> make container

This container is built from base image of [TensorRT](https://github.com/NVIDIA/TensorRT) version 19.06-py3, and [ONNX](https://github.com/onnx/onnx) IR version 0.0.3.

## Run docker container

```bash
# convert ONNX to TensorRT plan
docker run --runtime nvidia --rm -v /tmp/models:/models onnx-tensorrt:0.0.3-5.1.5 \
    onnx2trt -o /models/model.plan -v /models/model.onnx
# run TensorRT Inference Server on GPU
docker run --runtime nvidia --rm -e CUDA_VISIBLE_DEVICES=0 -v /tmp/models:/models \
    --shm-size=1g --ulimit memlock=-1 --ulimit stack=67108864 \
    -p 8000:8000 \
    nvcr.io/nvidia/tensorrtserver:19.06-py3 trtserver --model-store /models --strict-model-config false
# check status
curl localhost:8000/api/status
```
