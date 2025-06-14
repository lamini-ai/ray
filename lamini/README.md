#### Custom Ray images for Lamini

There are 3 images and 4 Dockerfiles.

- `-cpu` for Ray head and cpu worker.
- `-cuda` for Ray worker run on Nvidia's accelerators (abbreviated as cu124 for cuda + version).

- `-rocm` for Ray worker run on AMD's accelerators - Ray does not have an official image for ROCM.
  - base on vllm image, (used for now)
  - base on Ray cpu image. (need build and install vllm wheel)

You will need to copy ray source code changes in the corresponding dockerfile.

#### How to build: (example commands)

1. Change dockerfile as needed. Note that for `Dockerfile_rocm_base_vllm`, the path to ray is different as it builds on top of `vllm`.
1. Build and tag with base image info.
1. Push to Docker Hub.

``` sh
# build -cpu
docker build -f ./lamini/Dockerfile_cpu --platform=linux/amd64  -t powerml/ray:2.44.0-py312-cpu .

# build -cuda
docker build -f ./lamini/Dockerfile_cuda --platform=linux/amd64  -t powerml/ray:2.44.0-py312-cu124 .

# build -rocm
docker build -f ./lamini/Dockerfile_rocm_base_vllm --platform=linux/amd64  -t powerml/ray:2.44.0-py312-rocm .
```

``` sh
# push to Docker Hub
docker push powerml/ray:2.44.0-py312-cpu
docker push powerml/ray:2.44.0-py312-cu124
docker push powerml/ray:2.44.0-py312-rocm
```

Note: Dockerfile_rocm is currently unused but may be used in the future.
