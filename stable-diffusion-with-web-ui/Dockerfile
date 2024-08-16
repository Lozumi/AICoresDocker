# QingCloud Docker Distribution Service
# Stable Diffusion with Web UI
# Stable Diffusion Version 2.1 by Stability-AI
# Stable Diffusion Web UI v1.10.0 by AUTOMATIC1111

# Stage 1: Download necessary repositories using Alpine Linux with Git
FROM alpine/git:2.36.2 as download
MAINTAINER Lozumi<Lozumi.Everkine@gmail.com>

# Copy the shell script for cloning repositories
COPY clone.sh /clone.sh

# Clone the stable-diffusion-webui-assets repository
RUN . /clone.sh stable-diffusion-webui-assets https://github.com/AUTOMATIC1111/stable-diffusion-webui-assets.git 6f7db241d2f8ba7457bac5ca9753331f0c266917

# Clone the stable-diffusion-stability-ai repository, then remove unnecessary assets and data
RUN . /clone.sh stable-diffusion-stability-ai https://github.com/Stability-AI/stablediffusion.git cf1d67a6fd5ea1aa600c4df58e5b47da45f6bdbf \
  && rm -rf assets data/**/*.png data/**/*.jpg data/**/*.gif

# Clone additional repositories required by the project
RUN . /clone.sh BLIP https://github.com/salesforce/BLIP.git 48211a1594f1321b00f14c9f7a5b4813144b2fb9
RUN . /clone.sh k-diffusion https://github.com/crowsonkb/k-diffusion.git ab527a9a6d347f364e3d185ba6d714e22d80cb3c
RUN . /clone.sh clip-interrogator https://github.com/pharmapsychotic/clip-interrogator 2cf03aaf6e704197fd0dae7c7f96aa59cf1b11c9
RUN . /clone.sh generative-models https://github.com/Stability-AI/generative-models 45c443b316737a4ab6e40413d7794a7f5657c19f
RUN . /clone.sh stable-diffusion-webui-assets https://github.com/AUTOMATIC1111/stable-diffusion-webui-assets 6f7db241d2f8ba7457bac5ca9753331f0c266917

# Stage 2: Build the main environment using the PyTorch image
FROM pytorch/pytorch:2.3.0-cuda12.1-cudnn8-runtime
MAINTAINER Lozumi<Lozumi.Everkine@gmail.com>

# Set non-interactive mode for apt-get to avoid prompts during installation
ENV DEBIAN_FRONTEND=noninteractive PIP_PREFER_BINARY=1

# Install necessary system packages
RUN --mount=type=cache,target=/var/cache/apt \
  apt-get update && \
  # Required packages
  apt-get install -y fonts-dejavu-core rsync git jq moreutils aria2 \
  # Packages needed by extensions
  ffmpeg libglfw3-dev libgles2-mesa-dev pkg-config libcairo2 libcairo2-dev build-essential

# Set working directory
WORKDIR /

# Clone the stable-diffusion-webui repository and install its dependencies, of version 1.10.0
RUN --mount=type=cache,target=/root/.cache/pip \
  git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git && \
  cd stable-diffusion-webui && \
  git reset --hard v1.10.0 && \
  pip install -r requirements_versions.txt

# Define the root directory for the stable-diffusion-webui
ENV ROOT=/stable-diffusion-webui

# Copy cloned repositories from the download stage to the appropriate location
COPY --from=download /repositories/ ${ROOT}/repositories/
RUN mkdir ${ROOT}/interrogate && cp ${ROOT}/repositories/clip-interrogator/clip_interrogator/data/* ${ROOT}/interrogate

# Install additional Python packages
RUN --mount=type=cache,target=/root/.cache/pip \
  pip install pyngrok xformers==0.0.26.post1 \
  git+https://github.com/TencentARC/GFPGAN.git@8d2447a2d918f8eba5a4a01463fd48e45126a379 \
  git+https://github.com/openai/CLIP.git@d50d76daa670286dd6cacf3bcd80b5e4823fc8e1 \
  git+https://github.com/mlfoundations/open_clip.git@v2.20.0

# Install Google Performance Tools to address potential memory leaks
RUN apt-get -y install libgoogle-perftools-dev && apt-get clean
ENV LD_PRELOAD=libtcmalloc.so

# Copy local files to the Docker image
COPY . /docker

# Apply necessary patches and hacks
RUN \
  # Uncomment if needed: mv ${ROOT}/style.css ${ROOT}/user.css && \
  # Apply a workaround to set in_app_dir to True in Gradio's routes.py
  sed -i 's/in_app_dir = .*/in_app_dir = True/g' /opt/conda/lib/python3.10/site-packages/gradio/routes.py && \
  git config --global --add safe.directory '*'

# Set the working directory to the root of stable-diffusion-webui
WORKDIR ${ROOT}

# Expose the default port for the web UI
ENV NVIDIA_VISIBLE_DEVICES=all
ENV CLI_ARGS=""
EXPOSE 7860

# Define the entrypoint and default command for the container
ENTRYPOINT ["/docker/entrypoint.sh"]
CMD python -u webui.py --listen --port 7860 ${CLI_ARGS}
