# Use a lightweight base image with curl and git installed
FROM ubuntu:22.04

USER root

# Install dependencies
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    ca-certificates && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install WasmEdge
RUN curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash

# Set up environment for WasmEdge
RUN echo "source $HOME/.wasmedge/env" >> ~/.bashrc

# Make sure WasmEdge is available in the current session
RUN /bin/bash -c "source $HOME/.wasmedge/env && wasmedge --version"

# Set default command to bash
CMD ["/bin/bash"]
