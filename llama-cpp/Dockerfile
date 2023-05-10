FROM debian:stable-slim

RUN apt-get update \
    && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \ 
    git \ 
    build-essential \
    apt-transport-https \
    ca-certificates

RUN mkdir /app
WORKDIR /app

RUN useradd -u 1001 fun-llama \
    && chown -R fun-llama /app 
USER fun-llama

RUN git clone https://github.com/ggerganov/llama.cpp \
    && cd llama.cpp \
    && make

WORKDIR /app/llama.cpp

CMD ["/bin/bash"]
