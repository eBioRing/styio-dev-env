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
    && git clone https://github.com/eBioRing/styio.git
