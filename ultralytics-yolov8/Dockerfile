# QingCloud Distribution Service
# Ultralytics YOLOv8
# Image is CUDA-optimized for YOLOv8 single/multi-GPU training and inference

# Start FROM PyTorch image https://hub.docker.com/r/pytorch/pytorch or nvcr.io/nvidia/pytorch:23.03-py3
FROM pytorch/pytorch:2.3.1-cuda12.1-cudnn8-runtime
MAINTAINER Lozumi<Lozumi.Everkine@gmail.com>

# Set environment variables
# Avoid DDP error "MKL_THREADING_LAYER=INTEL is incompatible with libgomp.so.1 library" https://github.com/pytorch/pytorch/issues/37377
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_BREAK_SYSTEM_PACKAGES=1 \
    MKL_THREADING_LAYER=GNU

# Downloads to user config dir
ADD https://github.com/ultralytics/assets/releases/download/v0.0.0/Arial.ttf \
    https://github.com/ultralytics/assets/releases/download/v0.0.0/Arial.Unicode.ttf \
    /root/.config/Ultralytics/

# Install linux packages
# g++ required to build 'tflite_support' and 'lap' packages, libusb-1.0-0 required for 'tflite_support' package
# libsm6 required by libqxcb to create QT-based windows for visualization; set 'QT_DEBUG_PLUGINS=1' to test in docker
RUN apt update \
    && apt install --no-install-recommends -y gcc git zip unzip wget curl htop libgl1 libglib2.0-0 libpython3-dev gnupg g++ libusb-1.0-0 libsm6

# Security updates
# https://security.snyk.io/vuln/SNYK-UBUNTU1804-OPENSSL-3314796
RUN apt upgrade --no-install-recommends -y openssl tar

# Create working directory
WORKDIR /ultralytics

# Copy contents and configure git
COPY . .
RUN sed -i '/^\[http "https:\/\/github\.com\/"\]/,+1d' .git/config
ADD https://github.com/ultralytics/assets/releases/download/v8.2.0/yolov8n.pt .

# Install pip packages
RUN python3 -m pip install --upgrade pip wheel
# Pin TensorRT-cu12==10.1.0 to avoid 10.2.0 bug https://github.com/ultralytics/ultralytics/pull/14239 (note -cu12 must be used)
RUN pip install -e ".[export]" "tensorrt-cu12==10.1.0" "albumentations>=1.4.6" comet pycocotools

# Run exports to AutoInstall packages
# Edge TPU export fails the first time so is run twice here
RUN yolo export model=tmp/yolov8n.pt format=edgetpu imgsz=32 || yolo export model=tmp/yolov8n.pt format=edgetpu imgsz=32
RUN yolo export model=tmp/yolov8n.pt format=ncnn imgsz=32
# Requires <= Python 3.10, bug with paddlepaddle==2.5.0 https://github.com/PaddlePaddle/X2Paddle/issues/991
RUN pip install "paddlepaddle>=2.6.0" x2paddle
# Fix error: `np.bool` was a deprecated alias for the builtin `bool` segmentation error in Tests
RUN pip install numpy==1.23.5
# Remove exported models
RUN rm -rf tmp
