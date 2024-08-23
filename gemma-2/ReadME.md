# Gemma 2

### **1. 基础镜像**

- **镜像**: `pytorch/pytorch:2.1.2-cuda11.8-cudnn8-runtime`
  - 基于 PyTorch 2.1.2，支持 CUDA 11.8 和 cuDNN 8 的运行时环境，适用于深度学习任务。

### **2. 切换到 root 用户**

- **`USER root`**: 切换到 root 用户，以确保具有安装系统依赖项的权限。

### **3. 安装工具**

- **环境变量**: `DEBIAN_FRONTEND=noninteractive`
  - 防止在安装过程中出现交互式提示。

- **安装系统工具**:
  - `apt-get update` 更新包索引。
  - 安装了常用的系统工具，如 `apt-utils`, `curl`, `wget`, 和 `git`，用于后续操作。

### **4. 安装库**

- **环境变量**: `PIP_ROOT_USER_ACTION=ignore`
  - 防止使用 root 用户时出现警告。

- **升级 `pip`**: 通过 `python -m pip install --upgrade pip` 将 `pip` 升级到最新版本。
- **安装 Python 库**:
  - `numpy==1.24.4`: 用于数值计算。
  - `sentencepiece==0.1.99`: 一个常用于 NLP 任务的分词工具。

### **5. 克隆仓库**

- **使用 `git clone` 自动克隆仓库**: 将项目的源码从 GitHub 仓库 `https://github.com/your-repo/your-project.git` 克隆到 `/workspace/gemma` 目录。

### **6. 设置工作目录**

- **`WORKDIR /workspace/gemma/`**: 将工作目录设为 `/workspace/gemma`，确保后续命令都在此目录下执行。

### **7. 安装项目依赖**

- **`pip install -e .`**: 使用 `pip` 从源码安装项目依赖。

### **8. 总结**

此 Dockerfile 构建了一个基于 PyTorch 的镜像，预安装了必要的工具和库，并自动克隆了 Gemma 项目代码，适用于深度学习任务的开发和部署。