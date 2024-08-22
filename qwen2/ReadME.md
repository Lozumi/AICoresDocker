# Qwen2

### **1. 基于的镜像与构建阶段**

此 Dockerfile 使用了多个构建阶段（multi-stage builds），其中每个阶段用于特定的任务，优化了构建过程和最终镜像的大小。

- **`ARG CUDA_VERSION=12.1.0`**: 定义了 CUDA 版本号，后续可以灵活调整 CUDA 版本。
- **基础镜像**: `nvidia/cuda:${CUDA_VERSION}-cudnn8-devel-ubuntu20.04`
  - **作用**: 这是一个支持 CUDA 和 cuDNN 的 NVIDIA 官方开发镜像，基于 Ubuntu 20.04。用于深度学习模型的开发与训练，支持 GPU 加速。

### **2. 安装的依赖**

#### **`base` 阶段**
- **系统依赖**:
  - 安装了一些基础工具：`git`, `git-lfs`, `python3`, `python3-pip`, `python3-dev`, `wget`, `vim`。
  - **特殊依赖**:
    - **`git-lfs`**: 用于处理大文件存储（LFS）的 Git 扩展。
  - **清理**: 移除了 `apt` 缓存以减少镜像大小。

- **其他配置**:
  - 创建了 Python 3 的符号链接，使得可以直接通过 `python` 命令调用 Python 3。
  - 安装并初始化了 `git-lfs`。

#### **`dev` 阶段**
- **工作目录**: 
  - 创建并切换到 `/data/shared/Qwen/` 目录，这是后续工作的主要路径。

#### **`bundle_req` 阶段**
- **Python 依赖**:
  - 安装了用于图神经网络的 `networkx` 和 PyTorch 相关库（`torch`, `torchvision`, `torchaudio`），并且指明使用 CUDA 12.1 版本的 PyTorch。
  - 安装了 `transformers`, `accelerate`, `tiktoken`, `einops`, `scipy`，这些是 NLP 和深度学习常用的库。

#### **`bundle_finetune` 阶段**
- **可选安装**:
  - 通过 `ARG BUNDLE_FINETUNE=true` 控制是否安装 Fine-tuning 相关的库和工具。
  - 如果启用，安装了 `deepspeed` 和 `peft`，支持全模型微调和低秩自适应微调（LoRA）。
  - 为了支持 Q-LoRA，安装了 `libopenmpi-dev`, `openmpi-bin`, `optimum`, `auto-gptq`, `autoawq`, `mpi4py`。

#### **`bundle_vllm` 阶段**
- **可选安装**:
  - 通过 `ARG BUNDLE_VLLM=true` 控制是否安装 vLLM（高性能语言模型推理引擎）相关的依赖。
  - 如果启用，安装了 `vllm` 和 `fschat`，并开启了相关模型工作器和 Web UI 支持。

#### **`bundle_flash_attention` 阶段**
- **可选安装**:
  - 通过 `ARG BUNDLE_FLASH_ATTENTION=true` 控制是否安装 Flash Attention 库。
  - 如果启用，安装了 `flash-attn`，用于优化 Transformer 模型中的注意力机制。

### **3. 最终阶段**

- **`final` 阶段**: 
  - 将 `sft` 和 `demo` 示例代码复制到当前目录。
  - 暴露了端口 `80`，以便外部访问服务。

### **总结**

此 Dockerfile 为深度学习和大语言模型的开发与微调构建了一个灵活且可扩展的环境。通过多阶段构建和条件安装，可以根据具体需求选择安装不同的库和工具，支持从模型开发、微调到推理的完整流程。