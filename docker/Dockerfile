ARG BASE_IMAGE=ubuntu:22.04

FROM ${BASE_IMAGE} as base

RUN export DEBIAN_FRONTEND=nointeractive \
    && apt update \
    && apt install --yes --no-install-recommends \
        gawk \
        wget \
        git-core \
        diffstat \
        unzip \
        texinfo \
        build-essential \
        gcc-multilib-*-linux-gnu \
        chrpath \
        socat \
        libsdl1.2-dev \
    && apt install --yes --no-install-recommends \
        cpio \
        file \
        lz4 \
        liblz4-tool \
        zstd \
    && apt install --yes --no-install-recommends \
        locales \
        ca-certificates \
    && apt install --yes --no-install-recommends \
        python3 \
        tmux \
        screen \
        xterm \
        bmap-tools \
    && apt autoremove --yes \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

RUN locale-gen en_US \
    && locale-gen en_US.UTF-8 \
    && update-locale \

ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

ARG USERNAME=builder
ARG GROUPNAME=${USERNAME}
ARG PASSWORD=${USERNAME}
# TODO
ARG UID=1000
ARG GID=1000

RUN groupadd -g ${GID} ${GROUPNAME} \
    && useradd \
        -m \
        -s /bin/bash \
        -u ${UID} \
        -g ${GID} \
        ${USERNAME} \
    && echo ${USERNAME}:${PASSWORD} | chpasswd \
    && chown -R ${USERNAME} /home/${USERNAME}

USER ${USERNAME}
WORKDIR /home/${USERNAME}

ARG YOCTO_DISTRO=kirkstone

RUN git clone https://git.yoctoproject.org/git/poky \
        --depth 1 \
        --branch ${YOCTO_DISTRO}

RUN echo ". /home/${USERNAME}/poky/oe-init-build-env /home/${USERNAME}/build" >> ~/.bashrc
