# Use:
#
# `make container` will build a container
# `make save` will save the container as a tar ball

IMAGE_NAME ?=onnx-tensorrt
DOCKER_FILE ?=Dockerfile

file := 'VERSION'
VERSION := $(shell cat ${file})

container:
	docker build --pull \
	--build-arg TENSORRT_VERSION=19.06-py3 \
	--build-arg CUDA_VERSION=10.1 \
	-t $(IMAGE_NAME) -f ${DOCKER_FILE} .
	docker tag $(IMAGE_NAME) $(IMAGE_NAME):$(VERSION)

save: container
	docker save -o $(IMAGE_NAME).tar $(IMAGE_NAME)

run: container
	docker run --runtime nvidia --rm -it \
	$(IMAGE_NAME):$(VERSION) /bin/bash
