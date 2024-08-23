# Fish Speech

### **1. 基础镜像**

- **镜像**: `python:3.10.14-bookworm`
  - 基于 Debian Bookworm 的 Python 3.10.14 镜像，提供 Python 环境和相关工具。

### **2. 安装系统依赖**

- **环境变量**:
  - **`DEBIAN_FRONTEND=noninteractive`**: 防止 `apt-get` 安装过程中出现交互式提示。
- **命令**:
  - 使用 `apt-get` 安装一系列系统依赖和工具：
    - `git`, `curl`, `build-essential`, `ffmpeg`, `libsm6`, `libxext6`, `libjpeg-dev`, `zlib1g-dev`, `aria2`, `zsh`, `openssh-server`, `sudo`, `protobuf-compiler`, `cmake`, `libsox-dev`。
  - 清理缓存并删除 `apt-get` 的列表文件，以减小镜像体积：
    ```bash
    RUN apt-get update && apt-get install -y git curl build-essential ffmpeg libsm6 libxext6 libjpeg-dev \
        zlib1g-dev aria2 zsh openssh-server sudo protobuf-compiler cmake libsox-dev && \
        apt-get clean && rm -rf /var/lib/apt/lists/*
    ```

### **3. 安装 `oh-my-zsh`**

- **命令**:
  - 通过 `curl` 安装 `oh-my-zsh`，一个用于美化 Zsh 终端的工具：
    ```bash
    RUN sh -c "$(curl https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" "" --unattended
    ```

### **4. 设置 Zsh 为默认 Shell**

- **命令**:
  - 将 Zsh 设置为默认的 shell，并更新环境变量 `SHELL`：
    ```bash
    RUN chsh -s /usr/bin/zsh
    ENV SHELL=/usr/bin/zsh
    ```

### **5. 配置 `torchaudio`**

- **命令**:
  - 使用 `pip3` 安装 `torch`, `torchvision` 和 `torchaudio`，指定 PyTorch 官方的 CUDA 11.2 镜像源：
    ```bash
    RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
    ```

### **6. 克隆仓库**

- **命令**:
  - 使用 `git clone` 从 GitHub 上自动克隆 `fish-speech` 仓库到 `/fish_speech` 目录：
    ```bash
    RUN git clone https://github.com/fishaudio/fish-speech.git /fish_speech
    ```

### **7. 设置工作目录**

- **命令**:
  - 将工作目录设置为 `/fish_speech`，确保所有后续操作都在该目录下进行：
    ```bash
    WORKDIR /fish_speech
    ```

### **8. 安装项目依赖**

- **命令**:
  - 使用 `pip3` 安装项目中的 Python 依赖，以开发模式运行：
    ```bash
    RUN pip3 install -e .
    ```

### **9. 启动命令**

- **命令**:
  - 设置容器启动时默认运行 `zsh` 终端：
    ```bash
    CMD /bin/zsh
    ```

### **总结**

此 Dockerfile 构建了一个基于 Python 的开发环境，包含了所需的系统工具和库，自动克隆了 `fish-speech` 仓库，并安装了相关的 Python 依赖。配置了 `zsh` 作为默认 shell，并在容器启动时提供一个交互式终端。