FROM debian:12

RUN apt update && apt install -y apt-transport-https ca-certificates \ 
  && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware \n" >> /etc/apt/sources.list \
  && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware \n" >> /etc/apt/sources.list \
  && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware \n" >> /etc/apt/sources.list \
  && echo "deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware \n" >> /etc/apt/sources.list \
  && apt update

ARG LLVM_VERSION=18
ARG WORK_SPACE=/root

WORKDIR ${WORK_SPACE}

RUN apt install -y \
      lsb-release \
      software-properties-common \
      curl wget \
      vim git \
      rsync \
      gnupg \
    # [CMake]
    && apt install -y cmake \
    && apt install -y libssl-dev libcurl4-openssl-dev \
    # [LLVM: zstd]
    && apt install -y libzstd-dev libdwarf-dev \
    # [LLVM]
    && wget https://apt.llvm.org/llvm.sh \
    && chmod +x llvm.sh \
    && ./llvm.sh ${LLVM_VERSION} all \
    # [ICU]
    && apt install -y libicu67 libicu-dev 

RUN apt update \
    # [Python3]
    && apt install -y python3 python3-pip \
    # [LLVM - Lit]
    && pip install lit

# [System Environment Variables]
ENV clang=/usr/bin/clang-${LLVM_VERSION}
ENV clang++=/usr/bin/clang++-${LLVM_VERSION}

# [System Environment Variables]: CMake
ENV CC=/usr/bin/clang-${LLVM_VERSION}
ENV CXX=/usr/bin/clang++-${LLVM_VERSION}

# [Alias]
RUN echo "alias clang=/usr/bin/clang-${LLVM_VERSION}" >>/root/.bashrc
RUN echo "alias clang++=/usr/bin/clang++-${LLVM_VERSION}" >>/root/.bashrc
RUN ln /usr/bin/FileCheck-${LLVM_VERSION} /usr/bin/FileCheck
    
WORKDIR ${WORK_SPACE}/styio
