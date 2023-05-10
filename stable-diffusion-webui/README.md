# stable-diffusion-webui

[stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) containerized, with NVIDIA GPU support!

## Build

To enable NVIDIA GPU support with Docker, 
first setup [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html).

Unfortunately, as mentioned in the [docs](https://github.com/NVIDIA/nvidia-docker/wiki/Advanced-topics#default-runtime), 
the only way to access the GPU during build is by setting the Docker `default-runtime` to `nvidia`. So please do that.

```bash
docker build . -t stable-diffusion-webui
```

This will take some time, considering the size of the packages like PyTorch. Grab a cup of coffee!

## Run

Assuming you already have some Stable Diffusion models available, you can run:

```bash
MODELS_PATH="<SOME_ABSOLUTE_PATH_ON_HOST>"
VAE_PATH="<SOME_ABSOLUTE_PATH_ON_HOST>"
docker run --rm -it --name stable-diffusion-webui \
    -p 127.0.0.1:7860:7860 \
    --mount type=bind,source=$MODELS_PATH,target=/app/stable-diffusion-webui/models/Stable-diffusion,readonly \
    --mount type=bind,source=$VAE_PATH,target=/app/stable-diffusion-webui/models/VAE,readonly \
    --entrypoint "/app/stable-diffusion-webui/webui.sh" \
    stable-diffusion-webui:latest \
    --listen \
    --xformers \
    --force-enable-xformers
```

For example:

```bash
MODELS_PATH="$(pwd)/models"
VAE_PATH="$(pwd)/vae"
docker run --rm -it --name stable-diffusion-webui \
    -p 127.0.0.1:7860:7860 \
    --mount type=bind,source=$MODELS_PATH,target=/app/stable-diffusion-webui/models/Stable-diffusion,readonly \
    --mount type=bind,source=$VAE_PATH,target=/app/stable-diffusion-webui/models/VAE,readonly \
    --entrypoint "/app/stable-diffusion-webui/webui.sh" \
    stable-diffusion-webui:latest \
    --listen \
    --xformers \
    --force-enable-xformers
```

On the host, you can access `localhost:7860` on the host to use the application. We make use of the 
[xformers](https://github.com/facebookresearch/xformers) package for better optimized memory usage.

In a similar way, you can run other commands. Please note that there is no persistence for these containers, 
so any data created during the execution of the container will be lost. But this can be easily added using 
[volumes](https://docs.docker.com/storage/volumes/) or [bind mounts](https://docs.docker.com/storage/bind-mounts/).
