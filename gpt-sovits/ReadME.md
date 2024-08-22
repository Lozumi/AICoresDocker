# GPT-SoVITS with 

### **1. 基础镜像**

- **镜像**: `cnstark/pytorch:2.0.1-py3.9.17-cuda11.8.0-ubuntu20.04`
  - 基于带有 CUDA 11.8.0 和 Python 3.9.17 的 Ubuntu 20.04，适用于 PyTorch 2.0.1 的环境，支持 GPU 加速计算。

### **2. 标签信息**

- **`maintainer="breakstring@hotmail.com"`**: 维护者的联系方式。
- **`version="dev-20240209"`**: Docker 镜像的版本号。
- **`description="Docker image for GPT-SoVITS"`**: 镜像的描述，说明该镜像用于 GPT-SoVITS 项目。

### **3. 环境设置**

- **`DEBIAN_FRONTEND=noninteractive`**: 防止安装包时的交互式提示。
- **`TZ=Etc/UTC`**: 设置系统时区为 UTC。

### **4. 安装依赖**

- 通过 `apt-get` 安装以下第三方应用：
  - `tzdata`, `ffmpeg`, `libsox-dev`, `parallel`, `aria2`, `git`, `git-lfs`。
- 配置 `git lfs`。

### **5. 安装 Python 依赖**

- 复制 `requirements.txt` 文件到 `/workspace/`，并通过 `pip` 安装 Python 依赖库。

### **6. 定义构建参数**

- **`ARG IMAGE_TYPE=full`**: 定义一个构建时参数 `IMAGE_TYPE`，默认为 `full`，用于控制镜像的构建内容。

### **7. 基于条件的构建逻辑**

- 总是复制 Docker 目录的内容到 `/workspace/Docker`，但只有在 `IMAGE_TYPE` 不是 `elite` 时，执行下载脚本：
  - 运行 `download.sh` 和 `download.py` 脚本，下载所需模型和数据文件。
  - 安装 NLP 处理所需的 NLTK 数据包。

### **8. 克隆项目仓库**

- 使用 `git clone` 命令自动克隆了 GPT-SoVITS 的代码仓库到 `/workspace` 目录。

### **9. 复制项目文件**

- 复制项目的所有文件到 `/workspace`。

### **10. 暴露端口**

- 暴露了 9871, 9872, 9873, 9874, 9880 五个端口，以便服务监听。

### **11. 容器启动命令**

- 使用 `python webui.py` 作为默认的启动命令，启动 GPT-SoVITS 项目的 Web UI。

### **总结**

这个 Dockerfile 旨在构建 GPT-SoVITS 项目的环境，自动下载依赖和模型，并配置环境以支持基于 PyTorch 和 CUDA 的高性能计算。