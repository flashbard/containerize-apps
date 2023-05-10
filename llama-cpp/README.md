# llama-cpp

[llama-cpp](https://github.com/ggerganov/llama.cpp) containerized! Pure CPU based build.

## Build

Please ensure that your system meets the [pre-requisites](https://github.com/ggerganov/llama.cpp/blob/master/README.md#memorydisk-requirements).

```bash
docker build . -t llama-cpp
```

## Run

Assuming you have the ggml models available, you can run the following 
[command](https://github.com/ggerganov/llama.cpp/blob/master/README.md#interactive-mode) to start the application in
interactive mode:


```bash
MODELS_PATH_ON_HOST="<SOME_ABSOLUTE_PATH>"
MODEL_PATH_IN_CONTAINER="<SOME_ABSOLUTE_PATH>"
docker run --rm -it --name llama-cpp \
    --mount type=bind,source=$MODELS_PATH_ON_HOST,target=/app/llama.cpp/models/mounted,readonly \
    --entrypoint "./main" \
    llama-cpp:latest \
    -m $MODEL_PATH_IN_CONTAINER \
    -n 256 --repeat_penalty 1.0 --color -i -r "User:" -f prompts/chat-with-bob.txt

```

For examples, paths could look like:

```bash
MODELS_PATH_ON_HOST="$(pwd)/models"
MODEL_PATH_IN_CONTAINER="/app/llama.cpp/models/mounted/ggml-model-q4_0.bin"
```

Please note that it takes a few seconds before you're able to start typing in commands.

In a similar way, you can run other commands. Please note that there is no persistence for these containers, 
so any data created during the execution of the container will be lost. But this can be easily added using 
[volumes](https://docs.docker.com/storage/volumes/) or [bind mounts](https://docs.docker.com/storage/bind-mounts/).

