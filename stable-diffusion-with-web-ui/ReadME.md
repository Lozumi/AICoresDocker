# Stable Diffusion with Web UI

### **1. 基于的镜像**

该 `Dockerfile` 使用了多阶段构建，共有两个阶段：

- **阶段1: 下载必要的仓库**
  
  - **基础镜像**: `alpine/git:2.36.2`
    
    Alpine 是一个体积小、仅包含必要组件的 Linux 发行版，适用于轻量级操作。
  
- **阶段2: 构建运行环境**
  - **基础镜像**: `pytorch/pytorch:2.3.0-cuda12.1-cudnn8-runtime`
    
    PyTorch 运行时镜像，支持 CUDA 12.1 和 cuDNN 8，用于在 NVIDIA GPU 上运行 PyTorch。它提供了构建深度学习模型的基本环境。

### **2. 安装的依赖**

在第二阶段，该 Dockerfile 安装了多种系统依赖和 Python 库：

- **系统依赖**:
  - `fonts-dejavu-core`, `rsync`, `git`, `jq`, `moreutils`, `aria2`
    
    提供基础的系统工具和字体。
  - `ffmpeg`, `libglfw3-dev`, `libgles2-mesa-dev`, `pkg-config`, `libcairo2`, `libcairo2-dev`, `build-essential`
    
    支持图形处理、视频处理和构建依赖的开发工具。
  
- **Python 依赖**:
  - 在 `stable-diffusion-webui` 中通过 `requirements_versions.txt` 文件安装的库。
  - 额外安装的 Python 包包括：
    - `pyngrok`, `xformers==0.0.26.post1`
    - `GFPGAN`, `CLIP`, `open_clip`
    - 这些库用于图像生成、扩展功能支持和优化。

### **3. 配置与定制**

- **环境变量**:
  - `DEBIAN_FRONTEND=noninteractive`: 设置为非交互模式以防止安装软件包时出现提示。
  - `PIP_PREFER_BINARY=1`: 优先使用二进制格式安装 Python 包。
  - `ROOT=/stable-diffusion-webui`: 定义 Web UI 的根目录。
  - `NVIDIA_VISIBLE_DEVICES=all`: 允许 Docker 容器访问所有可用的 GPU。
  - `CLI_ARGS=""`: 用于传递额外的命令行参数。

- **优化与修复**:
  - 安装 `libgoogle-perftools-dev` 和配置 `LD_PRELOAD=libtcmalloc.so`，解决潜在的内存泄漏问题。
  - 使用 `sed` 修改 Gradio 的配置文件，解决路径问题。
  - `git config --global --add safe.directory '*'` 用于防止 Git 在容器中处理仓库时出现安全警告。

- **容器入口点与命令**:
  - 入口点设置为 `/docker/entrypoint.sh` 脚本。
  - 默认命令执行 `python -u webui.py --listen --port 7860 ${CLI_ARGS}` 启动 Web UI 服务。

### **总结**

该 `Dockerfile` 构建了 Stable Diffusion Web UI 运行环境，支持 GPU 加速。

## 参考

参考了[AbdBarho/stable-diffusion-webui-docker](https://github.com/AbdBarho/stable-diffusion-webui-docker/tree/master)。