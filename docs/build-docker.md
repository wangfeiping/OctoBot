# 构建 Docker 镜像

## 基本构建命令

docker build -t octobot:dev-v2.0.14 .  

带 TENTACLES_URL_TAG 参数构建（可选）：  
docker build --build-arg TENTACLES_URL_TAG=<tag> -t octobot:dev-v2.0.14 .  

## 构建流程说明

Dockerfile 使用多阶段构建：

```text
  阶段 1（base）：
  - 基础镜像：python:3.10-slim-buster
  - 安装编译依赖（gcc、libffi、openssl 等）
  - 创建 Python 虚拟环境 /opt/venv
  - 拷贝源码，执行 pip install -r requirements.txt 和 python setup.py install

  阶段 2（运行时）：
  - 从 base 阶段只复制 /opt/venv，减小最终镜像体积
  - 安装运行时依赖（cloudflared、图像处理库等）
  - 暴露端口 5001
  - 入口点：./docker-entrypoint.sh

  注意： docker-compose.yml 默认使用官方镜像 drakkarsoftware/octobot:stable，从源码构建需直接使用 docker build。
```

