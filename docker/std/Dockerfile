FROM debian:latest

RUN apt update \
    && apt install -y \
        lsb-release \
        software-properties-common \
        curl wget \
        vim git \
        gnupg \
    # [CMake]
    && apt install -y cmake \
    # [LLVM]: Version 16
    && wget https://apt.llvm.org/llvm.sh \
    && chmod +x llvm.sh \
    && ./llvm.sh 16 \
    # [Python3]
    && apt install -y python3 python3-pip \
    # [Node.js]: nvm
    && curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash \
    && source ~/.bashrc \
    && nvm install node --lts \
    # [Node.js]: yarn
    && apt install -y yarn