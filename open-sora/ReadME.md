# Open-Sora

### **1. 基础镜像**

- `FROM hpcaitech/pytorch-cuda:2.1.0-12.1.0`

  该镜像基于 PyTorch，并且支持 CUDA 12.1。这意味着镜像中已经包含了用于 GPU 加速的必要组件，如 CUDA 和 cuDNN，从而能够运行深度学习模型。

### **2. 镜像元数据**

- `LABEL org.opencontainers.image.source = "https://github.com/hpcaitech/Open-Sora"`
- `LABEL org.opencontainers.image.licenses = "Apache License 2.0"`
- `LABEL org.opencontainers.image.base.name = "docker.io/library/hpcaitech/pytorch-cuda:2.1.0-12.1.0"`

  - 这些标签提供了镜像的来源、许可信息以及基础镜像的元数据。

### **3. 设置工作目录**

- `WORKDIR /workspace/Open-Sora`

  - 设置容器内的工作目录为 `/workspace/Open-Sora`。接下来的操作都将在这个目录下进行。

### **4. 拷贝文件**

- `COPY . .`

  - 将当前目录的所有内容复制到容器的工作目录 `/workspace/Open-Sora`。

### **5. 安装系统依赖**

- `RUN apt-get update && apt-get install ffmpeg libsm6 libxext6 -y`

  - 更新包管理器并安装 `ffmpeg`、`libsm6`、`libxext6`。这些依赖通常用于图像处理和视频处理功能。

### **6. 安装 Python 库**

- `RUN pip install flash-attn --no-build-isolation`

  - 安装 `flash-attn`，这是一个用于加速注意力机制的库。

- `RUN pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --config-settings "--build-option=--cpp_ext" --config-settings "--build-option=--cuda_ext" git+https://github.com/NVIDIA/apex.git`

  - 安装 NVIDIA 的 Apex 库，用于混合精度训练。这里通过 `--cpp_ext` 和 `--cuda_ext` 选项来启用其 CUDA 扩展。

- `RUN pip install xformers --index-url https://download.pytorch.org/whl/cu121`

  - 安装 `xformers`，一个用于高效 Transformer 模型的库，特别适用于大规模深度学习任务。

### **7. 安装项目**

- `RUN pip install -v .`

  - 最后，安装当前项目中的 Python 包，使得项目依赖能够正确配置并在容器中运行。

### **总结**

这个 Dockerfile 构建了一个适用于 Open-Sora 项目的运行环境，重点是利用 GPU 加速来处理深度学习任务，并包含了一些关键的库，如 flash-attn 和 Apex，以优化模型的性能。