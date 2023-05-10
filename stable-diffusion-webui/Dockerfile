FROM nvidia/cuda:11.8.0-base-ubuntu22.04

RUN apt-get update \
    && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \
    git \
    apt-transport-https \
    ca-certificates \
    wget \
    python3 \
    python3-venv \
    libglib2.0-0 \
    libsm6 \
    libxrender1 \
    libxext6 \
    libgl1

RUN mkdir /app
WORKDIR /app

RUN useradd -u 1001 cool-sd && chown -R cool-sd /app \
    && mkdir /home/cool-sd && chown -R cool-sd /home/cool-sd
USER cool-sd

RUN wget https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui/master/webui.sh \
    && chmod +x ./webui.sh \
    && ./webui.sh --exit --xformers

WORKDIR /app/stable-diffusion-webui

# Workaround for Pytorch 2.0 + cu118 libnvrtc not found issue
# See https://discuss.pytorch.org/t/could-not-load-library-libcudnn-cnn-infer-so-8/175139
RUN cd venv/lib/python3.10/site-packages/torch/lib \
    && ln -s libnvrtc-672ee683.so.11.2 libnvrtc.so

EXPOSE 7860

CMD ["/bin/bash"]

