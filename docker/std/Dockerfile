FROM debian:latest

ARG LLVM_VERSION=16
ARG WORK_SPACE=/root

# [Alias]
ENV clang=/usr/bin/clang-${LLVM_VERSION}
ENV clang++=/usr/bin/clang++-${LLVM_VERSION}

# [CMake]
ENV CC=/usr/bin/clang-${LLVM_VERSION}
ENV CXX=/usr/bin/clang++-${LLVM_VERSION}

WORKDIR ${WORK_SPACE}

RUN apt update \
    && apt install -y \
        lsb-release \
        software-properties-common \
        curl wget \
        vim git \
        rsync \
        gnupg \
    # [CMake]
    && apt install -y cmake \
    && apt install -y libssl-dev libcurl4-openssl-dev \
    # [LLVM]
    && wget https://apt.llvm.org/llvm.sh \
    && chmod +x llvm.sh \
    && ./llvm.sh ${LLVM_VERSION} \
    # [ICU]
    && apt install -y libicu \
    && apt install -y libicu-dev \
    # [Python3]
    && apt install -y python3 python3-pip \
    # [Node.js]: nvm
    && curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash \
    && . ~/.bashrc \
    && nvm install node --lts \
    # [Node.js]: yarn
    && apt install -y yarn \
    # [GitHub Client]
    && curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
    && chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
    && apt update \
    && apt install gh -y \
    # [Styio]
    && git clone https://github.com/eBioRing/styio.git \
    # [Extra]
    && git clone https://github.com/eBioRing/styio-dev-env.git \
    && rsync -a styio-dev-env/docker/extra/.vscode ${WORK_SPACE} \
    && rm -rf styio-dev-env
    
WORKDIR ${WORK_SPACE}/styio
