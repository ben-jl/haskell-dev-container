FROM ubuntu:20.04

ARG DEBIAN_FRONTEND="noninteractive"

ARG INSTALL_ZSH="true"
ARG UPGRADE_PACKAGES="true"
ARG USERNAME=user1
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
COPY library-scripts/*.sh /tmp/library-scripts/
RUN yes | unminimize 2>&1 \ 
    && bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" \
    && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

ENV TZ=US/Central
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    libicu-dev \
    libtinfo-dev \
    libgmp-dev \
    zlib1g-dev \
    git \
    netbase

RUN apt-get install -y --no-install-recommends \
    ghc \
    build-essential \
    haskell-stack

RUN stack upgrade
RUN stack path --local-bin
ENV PATH="/root/.local/bin:${PATH}:.languageServer"
RUN mkdir .languageServer \
    && cd .languageServer \
    && wget https://github.com/haskell/haskell-language-server/releases/download/0.9.0/haskell-language-server-Linux-0.9.0.tar.gz \
    && tar -xf haskell-language-server-Linux-0.9.0.tar.gz
    
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

ENV DEBIAN_FRONTEND=dialog
ENV SHELL /bin/bash
USER $USERNAME
