# AI大模型基础镜像构造

## 任务概述

构建符合要求的镜像环境，用Dockerfile描述。

## 任务清单

| 模型镜像                        | 介绍                                                         |      | 实现方案 | 状态 |  |
| ------------------------------- | ------------------------------------------------------------ | ---- | -------- | ---- | ---- |
| Stable Diffusion 3 WebUI       | 多模态扩散变换器 (MMDiT) 文本到图像模型，启动器              |      | 构建一个基于Stable Diffusion Version 2.1和Stable Diffusion Web UI v1.10.0的Docker镜像。使用PyTorch官方镜像作为基础镜像，预装CUDA 12.1和cuDNN 8。 | 完成 |  |
| Stable Diffusion ComfyUl 工作流 | 基于 Stable Diffusion 的稳定扩散 GUI、api 和后端，带有图形/节点界面 |      | 构建一个基于Stable Diffusion Version 2.1和Comfy UI的Docker镜像。 | 完成 |  |
| Stable Diffusion Lora           | 基于SD的Iora训练                                             |      |          |      |      |
| Langchain-Chatchat              | 基于 Langchain 与 ChatGLM 等语言模型的本地知识库问答         |      | 构建一个基于Python的Docker镜像，从GitHub下载最新版源码。 | 完成 | [Langchain-Chatchat/docker/Dockerfile at master · chatchat-space/Langchain-Chatchat (github.com)](https://github.com/chatchat-space/Langchain-Chatchat/blob/master/docker/Dockerfile) |
| RVC                             | 语音转换训练推理                                             |      | 使用NVIDIA官方镜像作为基础镜像，预装 CUDA 11.6 和 cuDNN 8。从GitHub下载最新版源码。 | 完成 | [Retrieval-based-Voice-Conversion-WebUI/Dockerfile at main · RVC-Project/Retrieval-based-Voice-Conversion-WebUI (github.com)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/Dockerfile) |
| Qwen\_Qwen-7B                   | 通义千问大语言模型                                           |      | 使用NVIDIA官方镜像作为基础镜像，预装 CUDA 12.1 和 cuDNN 8。从GitHub下载最新版源码。 | 完成 | [Qwen2/docker/Dockerfile-cu121 at main · QwenLM/Qwen2 (github.com)](https://github.com/QwenLM/Qwen2/blob/main/docker/Dockerfile-cu121) |
| open-sora                       | 文生视频模型                                                 |      | 使用 HPC AI Tech官方镜像作为基础镜像，预装 CUDA 12.1和PyTorch 2.1.0。从GitHub下载最新版源码。 | 完成 | [Open-Sora/Dockerfile at main · hpcaitech/Open-Sora (github.com)](https://github.com/hpcaitech/Open-Sora/blob/main/Dockerfile) |
| Fooocus                         | $AI$绘画                                                     |      | 使用NVIDIA官方镜像作为基础镜像，预装 CUDA 12.4.1。从GitHub下载最新版源码。 | 完成 | [Fooocus/Dockerfile at main · lllyasviel/Fooocus (github.com)](https://github.com/lllyasviel/Fooocus/blob/main/Dockerfile) |
| GPT-SoVITS                      | 语音合成                                                     |      | 基于CNStark官方镜像作为基础镜像，预装 CUDA 11.8.0 、 Python 3.9.17 和 PyTorch 2.0.1。从GitHub下载最新版源码。 |      | [GPT-SoVITS/Dockerfile at main · RVC-Boss/GPT-SoVITS (github.com)](https://github.com/RVC-Boss/GPT-SoVITS/blob/main/Dockerfile) |
| Bert-VITS2                      | 文本转语音                                                   |      |          |      |      |
| YOLOv8         | 对象检测、图像分类和实例分割任务                             |          | 构建一个基于Ultralytics（YOLOv8）的Docker镜像。使用PyTorch官方镜像作为基础镜像，预装CUDA 12.1和cuDNN 8。适用于在GPU上进行YOLOv8模型（预装）的训练和推理，预装了YOLOv8n版本。 |完成||
| gemma-2-27b-it | 谷歌大语言模型                                             |          |      |||
| LLaMA Factory | 开源的低代码大模型微调框架，集成了业界广泛使用的微调技术，支持通过Web UI界面零代码微调大模型。 |          |      |||
| LLaMA3 Chinese | 中文优化或训练的大型语言模型版本                             |          |      |||
| wasmedge       | 轻量级、高性能、可扩展的 WebAssembly(WASM)运行时，专为云原 生、边缘计算和去中心化应用设计 |          |      |||
| RWKV           | RWKV 是一个基于 Transformer 架构的轻量级中文预训练语言模型   |          |      |||
| FunAudioLLM    | FunAudioLLM由两大核心模型组成：SenseVoice和CosyVoice，分别专注于语音理解和语音生成中应用场景提供强大的技术支持。 |          |      |||