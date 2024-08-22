# LangChain-Chatchat

### **1. 基于的镜像**

- **基础镜像**: `python:3.11`
  - **作用**: 这是一个官方的 Python 3.11 镜像，适用于需要最新 Python 版本的应用程序开发。此镜像包含 Python 解释器及其依赖，用于安装和运行 Python 应用程序。

### **2. 安装的依赖**

- **系统依赖**:
  - **时区设置**: 将容器的时区设置为 `Asia/Shanghai`。
  - **安装包管理器**: 使用 `apt-get` 更新包管理器并安装必要的依赖，如 `git`、`libgl1` 和 `libglib2.0-0`，这些依赖可能用于图形处理或 GUI 操作。

- **Python 依赖**:
  - **pip 和 setuptools**: 更新了 `pip` 和 `setuptools`，确保安装 Python 包时使用最新版本的工具。
  - **pipx 和 poetry**: 安装 `pipx`（一个用于隔离 Python 工具的实用程序），并通过 `pipx` 安装 `poetry`，这是一个用于 Python 项目依赖管理和打包的工具。

### **3. 配置与定制**

- **环境变量**:
  - `CHATCHAT_ROOT`: 定义了 `chatchat_data` 目录的路径，用于存储数据。
  - `PATH`: 添加 `poetry` 的安装路径到系统路径，确保可以全局调用 `poetry`。

- **项目代码下载与依赖安装**:
  - 通过 `git` 克隆 `Langchain-Chatchat` 项目。
  - 使用 `poetry` 安装项目依赖，包含 `lint` 和 `test` 配置，同时启用 `xinference` 额外依赖。

- **PYTHONPATH 设置**:
  - 将 `chatchat-server` 路径添加到 `PYTHONPATH`，确保 Python 能够找到 `chatchat` 模块。

### **4. 初始化与运行**

- **项目初始化**:
  - 在 `chatchat-server/chatchat` 目录下运行 `python cli.py init`，执行初始化脚本。
  - 添加并解压 `data.tar.gz` 到 `CHATCHAT_ROOT`，以初始化知识库文件。

- **暴露的端口**:
  - 暴露端口 `7861` 和 `8501`，以便外部访问 Web 服务和应用程序。

- **容器入口点**:
  - 使用 `python cli.py start -a` 作为容器的入口点，启动 `Langchain-Chatchat` 服务。

### **总结**

该 `Dockerfile` 为 `Langchain-Chatchat` 项目构建了一个基于 Python 3.11 的环境，包含了必要的系统工具和 Python 依赖，确保服务能够顺利运行并提供支持 AI 驱动的聊天功能。