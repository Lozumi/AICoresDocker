FROM hpcaitech/pytorch-cuda:2.1.0-12.1.0

# metainformation
LABEL org.opencontainers.image.source="https://github.com/hpcaitech/Open-Sora"
LABEL org.opencontainers.image.licenses="Apache License 2.0"
LABEL org.opencontainers.image.base.name="docker.io/library/hpcaitech/pytorch-cuda:2.1.0-12.1.0"

# Set the working directory
WORKDIR /workspace/Open-Sora

# Clone the repository
RUN git clone https://github.com/hpcaitech/Open-Sora.git /workspace/Open-Sora

# Copy the current directory contents into the container at /workspace/Open-Sora
COPY . .

# Install library dependencies
RUN apt-get update && apt-get install -y ffmpeg libsm6 libxext6

# Install flash attention
RUN pip install flash-attn --no-build-isolation

# Install apex
RUN pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --config-settings "--build-option=--cpp_ext" --config-settings "--build-option=--cuda_ext" git+https://github.com/NVIDIA/apex.git

# Install xformers
RUN pip install xformers --index-url https://download.pytorch.org/whl/cu121

# Install this project
RUN pip install -v .
