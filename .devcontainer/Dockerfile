ARG VERSION=latest

FROM archlinux:${VERSION}

ARG USERNAME
ARG HOME_DIR=/home/${USERNAME}

# Install all the language specific packages
RUN pacman -Suy --noconfirm \
    git \
    gcc \
    go \
    aarch64-linux-gnu-gcc \
    musl \
    musl-aarch64 \
    make \
    bash-completion \
    aws-cli \
    jq \
    direnv \
    unzip \
    xxd \
    docker \
    docker-compose \
    # Install sudo for dev container updating and debugging
    sudo && \
    pacman -Scc

RUN useradd -ms /bin/bash ${USERNAME} && \
    usermod -aG docker ${USERNAME} && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Set run as user
USER ${USERNAME}
WORKDIR ${HOME_DIR}

# Set environment variables for Go
ENV PATH=$PATH:${HOME_DIR}/.local/bin

# Install Go extensions
RUN go install golang.org/x/lint/golint@latest && \
    go install golang.org/x/tools/cmd/gorename@latest && \
    go install golang.org/x/tools/gopls@latest && \
    go install github.com/go-delve/delve/cmd/dlv@latest && \
    go install honnef.co/go/tools/cmd/staticcheck@latest

# Configure direnv
RUN mkdir -p ${HOME_DIR}/.local/bin && \
    echo eval '"$(direnv hook bash)"' >> ${HOME_DIR}/.bashrc

# Install dprint
RUN curl -fsSL https://dprint.dev/install.sh | sh && \
    mv ${HOME_DIR}/.dprint/bin/dprint ${HOME_DIR}/.local/bin/ && \
    rm -rf ${HOME_DIR}/.dprint

# Install Starship prompt
RUN curl -sS https://starship.rs/install.sh | sh -s -- --yes && \
    starship init bash >> ~/.bashrc

# Install Claude CLI
RUN curl -fsSL https://claude.ai/install.sh | bash
