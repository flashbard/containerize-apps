# llama-cpp

[llama-cpp](https://github.com/ggerganov/llama.cpp) containerized! Pure CPU based build.

# Build

Please ensure that your system meets the [pre-requisites](https://github.com/ggerganov/llama.cpp/blob/master/README.md#memorydisk-requirements).

```bash
docker build . -t llama-cpp
```

# Run

Assuming you have the ggml models available, you can run the following [command](https://github.com/ggerganov/llama.cpp/blob/master/README.md#prepare-data--run) to start the application:


```bash
MODELS_PATH_ON_HOST="<SOME_ABSOLUTE_PATH>"
MODEL_PATH_IN_CONTAINER="<SOME_ABSOLUTE_PATH>"
docker run --rm -it --name llama-cpp \
    --mount type=bind,source=$MODELS_PATH_ON_HOST,target=/app/llama.cpp/models/mounted,readonly \
    --entrypoint "./main" \
    llama-cpp:latest \
    -m $MODEL_PATH_IN_CONTAINER
    -n 128
```

For example:

```bash
MODELS_PATH_ON_HOST="$(pwd)/models"
MODEL_PATH_IN_CONTAINER="/app/llama.cpp/models/mounted/ggml-model-q4_0.bin"
docker run --rm -it --name llama-cpp \
    --mount type=bind,source=$MODELS_PATH_ON_HOST,target=/app/llama.cpp/models/mounted,readonly \
    --entrypoint "./main" \
    llama-cpp:latest \
    -m $MODEL_PATH_IN_CONTAINER
```

In a similar way, you can run other commands. Please note that there is no persistence for these containers, so any data created during the execution of the container will be lost. But this can be easily added using [volumes](https://docs.docker.com/storage/volumes/) or [bind mounts](https://docs.docker.com/storage/bind-mounts/).
