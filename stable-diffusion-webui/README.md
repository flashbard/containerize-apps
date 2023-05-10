# stable-diffusion-webui

[stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) containerized, with NVIDIA GPU support!

## Build

To enable NVIDIA GPU support with Docker, 
first setup [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html).

Unfortunately, as mentioned in the [docs](https://github.com/NVIDIA/nvidia-docker/wiki/Advanced-topics#default-runtime), 
the only way to access the GPU during build is by setting the Docker `default-runtime` to `nvidia`.

```bash
docker build . -t stable-diffusion-webui
```

This will take some time, considering the size of the packages like PyTorch. Grab a cup of coffee!

## Run

In a similar way, you can run other commands. Please note that there is no persistence for these containers, 
so any data created during the execution of the container will be lost. But this can be easily added using 
[volumes](https://docs.docker.com/storage/volumes/) or [bind mounts](https://docs.docker.com/storage/bind-mounts/).
