# Fooocus

### **1. 基础镜像**
- **镜像**: `nvidia/cuda:12.4.1-base-ubuntu22.04`
  - 基于带有 CUDA 12.4.1 的 Ubuntu 22.04，适用于 GPU 加速计算的环境。

### **2. 环境变量**
- **`DEBIAN_FRONTEND=noninteractive`**: 防止安装包时的交互式提示。
- **`CMDARGS="--listen"`**: 传递给启动命令的默认参数。

### **3. 安装依赖**
- 通过 `apt-get` 安装基本的系统依赖：
  - `curl`, `libgl1`, `libglib2.0-0`, `python3-pip`, `python-is-python3`, `git`。

### **4. 克隆仓库**
- 使用 `git clone` 命令自动克隆了 `https://github.com/lllyasviel/Fooocus.git` 仓库到 `/content/app` 目录。

### **5. 安装 Python 依赖**
- 复制并安装 `requirements_docker.txt` 和 `requirements_versions.txt` 中的 Python 依赖，然后删除临时文件。
- 单独安装 `xformers` 库，用于优化和扩展应用的功能。

### **6. 下载其他必要文件**
- 通过 `curl` 下载了 `gradio` 的 `frpc_linux_amd64` 文件并设置执行权限。这通常是为了在特定环境下启动 `gradio` 服务。

### **7. 创建用户并设置权限**
- 创建了一个名为 `user` 的新用户，并设置了 `/content` 目录的权限。

### **8. 设置工作目录和用户**
- 将工作目录设为 `/content/app` 并切换到 `user` 用户环境，以确保后续操作的安全性。

### **9. 移动和配置应用**
- 将默认的 `models` 目录移动到 `models.org`，这可能是为了防止覆盖或备份。

### **10. 入口点设置**
- 复制了 `entrypoint.sh` 脚本到 `/content/`，并使用该脚本作为容器启动时的入口点。

### **总结**
此 Dockerfile 旨在构建并运行 `Fooocus` 应用，利用 CUDA GPU 加速，并自动配置必要的依赖和环境。