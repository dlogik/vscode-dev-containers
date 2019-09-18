#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------
FROM python:2.7.16-buster

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Or your actual UID, GID on Linux if not the default 1000
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Configure apt and install packages
RUN sed -i 's/http:\/\/deb.debian.org\/debian/http:\/\/mirror.aarnet.edu.au\/debian/' /etc/apt/sources.list \
  && apt-get update \
  && apt-get -y install --no-install-recommends apt-utils dialog groff python python-pip python-setuptools \
  python3 python3-pip python3-setuptools 2>&1 \
  #
  # Verify git, process tools, lsb-release (common in install instructions for CLIs) installed
  && apt-get -y install git procps lsb-release \
  && apt-get install ffmpeg -y \
  && pip --disable-pip-version-check --no-cache-dir install boto3 pylint autopep8 \
  && pip3 --disable-pip-version-check --no-cache-dir install awscli \
  #
  # Create a non-root user to use if preferred - see https://aka.ms/vscode-remote/containers/non-root-user.
  && groupadd --gid $USER_GID $USERNAME \
  && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
  # [Optional] Uncomment the next three lines to add sudo support
  # && apt-get install -y sudo \
  # && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
  # && chmod 0440 /etc/sudoers.d/$USERNAME \
  #
  && apt-get autoremove -y \
  && apt-get clean -y \
  && rm -rf /var/lib/apt/lists/*

# Put awscli in path
RUN echo "export PATH=~/.local/bin:$PATH" >> /root/.bashrc

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=
