# build 指令
# docker build -t <image_name> .

# 使用 Ubuntu 22.04 作为基础镜像
# 默认的 python 是 python3.10
# 这里使用的系统镜像版本，会影响到能用哪些包，比如可用的 python 版本
# :（冒号）后面就是镜像的版本号
FROM ubuntu:22.04

# 避免安装过程中的交互式提示
ENV DEBIAN_FRONTEND=noninteractive

# 更新包列表并安装基础工具和 python 3.11
# python3.*-dev 包含 python 的头文件等内容
RUN apt-get update && apt-get install -y \
    python3.11 \
    python3.11-dev \
    python3.11-venv \
    && rm -rf /var/lib/apt/lists/*

# 设置 python3 指向 python3.11
# 设置 python 指向 python3.11
# 关于 update-alternatives 的用法:
# --install 创建一个新的替代项
# --remove 删除一个替代项
# --config 显示当前的替代项
# --query 查询替代项
# --set 设置替代项
# --list 列出所有替代项
# /usr/bin/python3 是系统中实际的[链接位置]，它是一个符号链接（软链接）
# lrwxrwxrwx 1 root root xx xxx xx xx:xx /usr/bin/python3 -> /usr/bin/python3.10
# python3 是替代项的名称
# /usr/bin/python3.11 是替代项的实际位置（实际执行文件）
# 2 是优先级，数字越大优先级越高
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 2 \
    && update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 1 \
    && update-alternatives --install /usr/bin/python python /usr/bin/python3.11 1

# 现在再安装pip
# 安装 python3-pip 的时候，是为系统的 python3 指向的 python3.11 安装的
RUN apt-get update && apt-get install -y \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# 后面我们都直接使用 python 命令了，指向的是 python3.11

# 升级pip
RUN python -m pip install --upgrade pip

# 安装numpy
RUN python -m pip install numpy

# 设置工作目录
WORKDIR /app

# 设置默认命令
# 如果在 docker run 的时候没有指定命令，则默认执行 python
# 如果指定了命令，则执行指定的命令，本命令就会被覆盖
# 如何在 run 的时候指定命令？
# docker run <image_name> python --version
CMD ["python"]

# CMD 和 RUN 的区别
# RUN 是在构建镜像的时候执行的，CMD 是在运行容器的时候执行的
# 一个 Dockerfile 中只能有一个 CMD 命令，如果有多个，则只有最后一个 CMD 会生效
# 如果需要执行多个命令，可以使用 ENTRYPOINT 和 CMD 的组合
# 比如：ENTRYPOINT ["python"] CMD ["--version"]

# 在 run 的时候是否需要 -it 选项？
# -i 表示交互式，-t 表示分配一个伪终端
# 运行 bash, python 等命令的时候，需要 -it 选项，相当于可以进入容器，执行命令
# 比如：docker run -it <image_name> bash
# 运行 python xxx.py 的时候，只需要等程序运行就可以了，不需要 -it 选项
# 比如：docker run <image_name> python xxx.py
