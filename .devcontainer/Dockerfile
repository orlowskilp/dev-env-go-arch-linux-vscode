ARG VERSION

FROM archlinux:${VERSION}

# Needs to be defined after FROM
ARG USERNAME

RUN useradd -ms /bin/bash ${USERNAME}
# Install all the language specific packages
RUN pacman -Suy --noconfirm \
    git \
    gcc \
    go \
# Install sudo for dev container updating and debugging
    sudo && \
    pacman -Scc
# Install extensions for VSCode
RUN go install github.com/sqs/goreturns@latest && \
    go install github.com/rogpeppe/godef@latest && \
    go install github.com/ramya-rao-a/go-outline@latest && \
    go install github.com/mdempsky/gocode@latest && \
    go install github.com/uudashr/gopkgs/v2/cmd/gopkgs@latest && \
    go install github.com/stamblerre/gocode@latest && \
    go install golang.org/x/lint/golint@latest && \
    go install golang.org/x/tools/cmd/gorename@latest && \
    go install golang.org/x/tools/gopls@latest && \
    go install github.com/go-delve/delve/cmd/dlv@latest && \
    go install honnef.co/go/tools/cmd/staticcheck@latest

# Move files to user directory
RUN cp -r /root/go /home/${USERNAME} && \
    chown -R ${USERNAME}. /home/${USERNAME}/go && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
# Set run as user
USER ${USERNAME}
WORKDIR /home/${USERNAME}