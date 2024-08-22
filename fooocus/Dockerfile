FROM nvidia/cuda:12.4.1-base-ubuntu22.04
ENV DEBIAN_FRONTEND=noninteractive
ENV CMDARGS="--listen"

# 安装必要的依赖
RUN apt-get update -y && \
    apt-get install -y curl libgl1 libglib2.0-0 python3-pip python-is-python3 git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 克隆仓库
RUN git clone https://github.com/lllyasviel/Fooocus.git /content/app

# 安装Python依赖
COPY requirements_docker.txt requirements_versions.txt /tmp/
RUN pip install --no-cache-dir -r /tmp/requirements_docker.txt -r /tmp/requirements_versions.txt && \
    rm -f /tmp/requirements_docker.txt /tmp/requirements_versions.txt

RUN pip install --no-cache-dir xformers==0.0.23 --no-dependencies

# 下载其他必要文件
RUN curl -fsL -o /usr/local/lib/python3.10/dist-packages/gradio/frpc_linux_amd64_v0.2 https://cdn-media.huggingface.co/frpc-gradio-0.2/frpc_linux_amd64 && \
    chmod +x /usr/local/lib/python3.10/dist-packages/gradio/frpc_linux_amd64_v0.2

# 创建用户并设置目录权限
RUN adduser --disabled-password --gecos '' user && \
    mkdir -p /content/app /content/data && \
    chown -R user:user /content

# 设置工作目录和用户
WORKDIR /content/app
USER user

# 移动和配置应用
RUN mv /content/app/models /content/app/models.org

# 定义容器的入口点
COPY entrypoint.sh /content/
CMD ["sh", "-c", "/content/entrypoint.sh ${CMDARGS}"]
