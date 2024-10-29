# Use an official Debian base image
FROM debian:trixie-slim

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update
RUN apt-get install -y git \
    build-essential \
    cmake \
    libreadline-dev \
    zlib1g-dev \
    libbz2-dev \
    libsqlite3-dev \
    libpcre2-dev \
    libcurl4-gnutls-dev \
    libarchive-dev \
    libncurses-dev \
    autoconf \
    locales \
    wireshark \
    tshark \
    pipx \
    re2c \
    cargo \
    gawk

# Set the locale, based on https://web.archive.org/web/20230323021946/http://jaredmarkell.com/docker-and-locales/
RUN sed -i '/en_US.UTF-8/s/^# //' /etc/locale.gen && \
    locale-gen
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

RUN pipx ensurepath && pipx install check-jsonschema

#########################################################################################################
# From https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user#_creating-a-nonroot-user
ARG USERNAME=devcontainer
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash -d /home/${USERNAME} $USERNAME  \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME
# End of vscode non-root user addition
#########################################################################################################

RUN echo "export MAKEFLAGS=-j$(nproc)" >> /home/${USERNAME}/.bashrc

WORKDIR /home/${USERNAME}
