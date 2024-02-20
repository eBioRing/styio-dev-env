FROM debian:12

RUN apt-get update && apt-get install -y apt-transport-https ca-certificates \ 
  && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware \n" >> /etc/apt/sources.list \
  && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware \n" >> /etc/apt/sources.list \
  && echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware \n" >> /etc/apt/sources.list \
  && echo "deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware \n" >> /etc/apt/sources.list \
  && apt update

ARG LLVM_VERSION=18
ARG WORK_SPACE=/root

WORKDIR ${WORK_SPACE}

RUN apt-get install -y \
    lsb-release software-properties-common \
    curl wget vim git \
    rsync gnupg \
  # [CMake]
  && apt-get install -y cmake \
  && apt-get install -y libssl-dev libcurl4-openssl-dev \
  # [ICU for Unicode]
  && apt-get install -y libicu-dev \
  # [zstd for LLVM]
  && apt-get install -y libzstd-dev libdwarf-dev \
  # [zlib for LLVM]
  && apt-get install -y zlib1g-dev

# [Python3]
RUN apt install -y pipx \
  &&  pipx ensurepath \
  # [lit for LLVM]
  && pipx install lit

# [LLVM]
RUN wget https://apt.llvm.org/llvm.sh \
  && chmod +x llvm.sh \
  && ./llvm.sh ${LLVM_VERSION} all 

# [System Environment Variables]
ENV clang=/usr/bin/clang-${LLVM_VERSION}
ENV clang++=/usr/bin/clang++-${LLVM_VERSION}

# [System Environment Variables]: CMake
ENV CC=/usr/bin/clang-${LLVM_VERSION}
ENV CXX=/usr/bin/clang++-${LLVM_VERSION}

# [Alias]
RUN echo "alias clang=/usr/bin/clang-${LLVM_VERSION}" >>/root/.bashrc \
  && echo "alias clang++=/usr/bin/clang++-${LLVM_VERSION}" >>/root/.bashrc

RUN ln /usr/bin/FileCheck-${LLVM_VERSION} /usr/bin/FileCheck \
  # clang frontend
  && ln /usr/bin/clang-${LLVM_VERSION} /usr/bin/clang \ 
  # clang++ frontend
  && ln /usr/bin/clang++-${LLVM_VERSION} /usr/bin/clang++ \ 
  # LLVM IR -> Bitcode (Executable) [file.ll -> file.bc]
  && ln /usr/bin/llvm-as-${LLVM_VERSION} /usr/bin/llvm-as \ 
  # LLVM Bitcode (Executable) -> LLVM IR [file.bc -> file.ll]
  && ln /usr/bin/llvm-dis-${LLVM_VERSION} /usr/bin/llvm-dis \ 
  # Link LLVM Bitcode [Link files.ll]
  && ln /usr/bin/llvm-link-${LLVM_VERSION} /usr/bin/llvm-link \ 
  # LLVM Bitcode (Executable) -> Specific Machine Code
  && ln /usr/bin/llc-${LLVM_VERSION} /usr/bin/llc \ 
  # Execute LLVM Bitcode (JIT) [Interpret file.ll]
  && ln /usr/bin/lli-${LLVM_VERSION} /usr/bin/lli 
