# syntax=docker/dockerfile:1
###############################################################################
# Use:
# - docker build --progress plain --tag ghcr.io/braincraftio/devcontainer:latest -f containers/devcontainer/Dockerfile .
# - docker run --rm -d --name braincraftio --hostname braincraftio ghcr.io/braincraftio/devcontainer:latest
###############################################################################
#
# BrainCraft.io Development Container
#
# Purpose: Unified development environment for AI-assisted development
# with 100% parity between local, CI/CD, and LLM contexts
#

# Use Ubuntu 24.04 LTS as base for maximum compatibility
FROM ubuntu:24.04 AS base
LABEL tag="braincraftio"
ENV DEVCONTAINER="braincraftio"

# Shell configuration for build process
SHELL ["/bin/bash", "-c"]
USER root

#################################################################################
# Environment Variables
#################################################################################

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC
ENV TERM=linux

# Disable LC_ALL for Nix compatibility
ENV LC_ALL=""

# Common Dockerfile Container Build Functions
ENV apt_update="apt-get update"
ENV apt_install="TERM=linux DEBIAN_FRONTEND=noninteractive apt-get install -q --yes --purge --assume-yes --auto-remove --allow-downgrades -o Dpkg::Options::='--force-confdef' -o Dpkg::Options::='--force-confold'"
ENV apt_clean="apt-get clean && apt-get autoremove -y && apt-get purge -y --auto-remove"
ENV curl="/usr/bin/curl --silent --show-error --tlsv1.2 --location"
ENV curl_github="/usr/bin/curl --silent --show-error --tlsv1.2 --request GET --url"
ENV dir_clean="\
  rm -rf \
  /var/lib/{apt,cache,log} \
  /usr/share/{doc,man,locale} \
  /var/cache/apt \
  /home/*/.cache \
  /root/.cache \
  /var/tmp/* \
  /tmp/* \
  "

# Binary installation defaults
ENV BIN="/usr/local/bin"
ENV INSTALL="install -m 755 -o root -g root"

#################################################################################
# Base system packages installation
#################################################################################

# Install base system dependencies - comprehensive list from both Dockerfiles
# SC2016: The 'eval "$(direnv hook bash)"' string is intentionally single-quoted to prevent expansion during Docker build
# hadolint ignore=SC2016
RUN echo \
  && apt-get update \
  && TERM=linux DEBIAN_FRONTEND=noninteractive apt-get install -q --yes --no-install-recommends --purge --assume-yes --auto-remove --allow-downgrades -o Dpkg::Options::='--force-confdef' -o Dpkg::Options::='--force-confold' \
    # Essential build tools
    build-essential \
    pkg-config \
    cmake \
    autoconf \
    automake \
    libtool \
    ninja-build \
    ccache \
    make \
    # Version control
    git \
    git-lfs \
    # Network tools
    curl \
    wget \
    netcat-openbsd \
    dnsutils \
    iputils-ping \
    socat \
    # Security tools
    gnupg \
    gnupg2 \
    openssh-client \
    ca-certificates \
    apt-transport-https \
    software-properties-common \
    dirmngr \
    lsb-release \
    # Shell and terminal
    bash \
    bash-completion \
    zsh \
    tmux \
    screen \
    # Text processing and editors
    jq \
    yq \
    bc \
    xmlstarlet \
    vim \
    nano \
    neovim \
    # Archive tools
    zip \
    unzip \
    tar \
    gzip \
    bzip2 \
    xz-utils \
    # System utilities
    sudo \
    locales \
    tzdata \
    htop \
    tree \
    file \
    less \
    # Development utilities
    direnv \
    httpie \
    ripgrep \
    fd-find \
    bat \
    eza \
    fzf \
    # Python build dependencies
    python3-dev \
    python3-pip \
    python3-venv \
    python3-setuptools \
    python3-virtualenv \
    python3-wheel \
    # Required for some Python packages
    libffi-dev \
    libssl-dev \
    # Required for some Node packages
    libnode-dev \
    node-gyp \
    # Documentation tools
    pandoc \
    graphviz \
    # Performance tools
    valgrind \
    heaptrack \
    # Fast linker for Rust
    lld \
    # Go development (initial install, will be managed by mise)
    golang-go \
    # Playwright browser dependencies
    libasound2t64 \
    libatk-bridge2.0-0t64 \
    libatk1.0-0t64 \
    libatspi2.0-0t64 \
    libcairo-gobject2 \
    libcairo2 \
    libcups2t64 \
    libdbus-1-3 \
    libdrm2 \
    libgbm1 \
    libgdk-pixbuf-2.0-0 \
    libglib2.0-0t64 \
    libgtk-3-0t64 \
    libgtk-3-common \
    libgtk-4-1 \
    libgtk-4-common \
    libnspr4 \
    libnss3 \
    libpango-1.0-0 \
    libpangocairo-1.0-0 \
    libx11-6 \
    libx11-xcb1 \
    libxcb1 \
    libxcb-shm0 \
    libxcomposite1 \
    libxdamage1 \
    libxext6 \
    libxfixes3 \
    libxi6 \
    libxkbcommon0 \
    libxrandr2 \
    libxrender1 \
    libxss1 \
    libxtst6 \
    # Additional Playwright dependencies
    gstreamer1.0-libav \
    gstreamer1.0-plugins-base \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-bad \
    libenchant-2-2 \
    libepoxy0 \
    libevent-2.1-7t64 \
    libflite1 \
    libgles2 \
    libgstreamer-gl1.0-0 \
    libgstreamer-plugins-bad1.0-0 \
    libgstreamer-plugins-base1.0-0 \
    libgstreamer1.0-0 \
    libharfbuzz-icu0 \
    libharfbuzz0b \
    libhyphen0 \
    libicu74 \
    libjpeg-turbo8 \
    liblcms2-2 \
    libmanette-0.2-0 \
    libopus0 \
    libpng16-16t64 \
    libsecret-1-0 \
    libsecret-common \
    libvpx9 \
    libwayland-client0 \
    libwayland-cursor0 \
    libwayland-egl1 \
    libwayland-server0 \
    libwebp7 \
    libwebpdemux2 \
    libwoff1 \
    libx264-164 \
    libxcb-xkb1 \
    libxcursor1 \
    libxinerama1 \
    libxkbcommon-x11-0 \
    libxkbfile1 \
    libxml2 \
    libxslt1.1 \
    # Fonts for browsers
    fonts-liberation \
    fonts-noto-color-emoji \
    fonts-unifont \
    fonts-ipafont-gothic \
    fonts-wqy-zenhei \
    fonts-tlwg-loma-otf \
    fonts-freefont-ttf \
    xfonts-cyrillic \
    xfonts-encodings \
    xfonts-scalable \
    xfonts-utils \
    # Additional browser dependencies
    libatomic1 \
    libegl1 \
    libevdev2 \
    libavif16 \
    xvfb \
    x11-xkb-utils \
    xserver-common \
  && echo 'eval "$(direnv hook bash)"' | tee -a /etc/bash.bashrc \
  && apt-get clean && apt-get autoremove -y && apt-get purge -y --auto-remove \
  && rm -rf \
    /var/lib/{apt,cache,log} \
    /usr/share/{doc,man,locale} \
    /var/cache/apt \
    /home/*/.cache \
    /root/.cache \
    /var/tmp/* \
    /tmp/* \
  && echo

#################################################################################
# Locale configuration
#################################################################################

# Generate and set locale
RUN echo \
  && locale-gen en_US.UTF-8 \
  && echo
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

#################################################################################
# User creation and sudo configuration
#################################################################################

# Create non-root user with sudo access
RUN echo \
  && (id ubuntu &> /dev/null || useradd -m -s /bin/bash ubuntu) \
  && usermod -aG sudo ubuntu \
  && echo "ubuntu ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
  && echo "ubuntu ALL=(ALL) NOPASSWD:ALL" | tee /etc/sudoers.d/ubuntu \
  && groupadd -f docker \
  && usermod -aG docker ubuntu \
  && echo

#################################################################################
# Create required directories with correct permissions
#################################################################################

RUN echo \
  && mkdir -p /home/ubuntu/.local/bin \
  && mkdir -p /home/ubuntu/.config \
  && mkdir -p /home/ubuntu/.cache/go-build \
  && mkdir -p /home/ubuntu/.cargo \
  && mkdir -p /home/ubuntu/.npm \
  && mkdir -p /home/ubuntu/.local/share/mise \
  && mkdir -p /home/ubuntu/.config/gh \
  && mkdir -p /home/ubuntu/.config/direnv \
  && mkdir -p /home/ubuntu/.krew/bin \
  && mkdir -p /opt/tools \
  && mkdir -p /opt/actions-runner \
  && mkdir -p /workspace \
  && mkdir -p /workspace/.npm-global \
  && mkdir -p /workspace/.pnpm-store \
  && mkdir -p /workspace/.node-modules-volumes \
  && mkdir -p /go/bin /go/pkg/mod /go/pkg/cache \
  && chown -R ubuntu:ubuntu /home/ubuntu /opt/tools /opt/actions-runner /workspace /go \
  && chmod -R 775 /home/ubuntu /workspace /go \
  && chmod -R 755 /opt/tools /opt/actions-runner \
  && echo

#################################################################################
# Create symlinks for tools with different Debian names
#################################################################################

RUN echo \
  && ln -s /usr/bin/fdfind /usr/local/bin/fd \
  && ln -s /usr/bin/batcat /usr/local/bin/bat \
  && echo

#################################################################################
# Install Docker CLI for Docker-outside-of-Docker
#################################################################################

RUN echo \
  && export NAME="docker" \
  && export TEST="${NAME} --version" \
  && echo "---------------------------------------------------------" \
  && echo "INFO[${NAME}] Installing Docker CLI" \
  && echo "---------------------------------------------------------" \
  && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null \
  && apt-get update \
  && TERM=linux DEBIAN_FRONTEND=noninteractive apt-get install -q --yes --no-install-recommends --purge --assume-yes --auto-remove --allow-downgrades -o Dpkg::Options::='--force-confdef' -o Dpkg::Options::='--force-confold' docker-ce-cli docker-compose-plugin docker-buildx-plugin \
  && apt-get clean && apt-get autoremove -y && apt-get purge -y --auto-remove \
  && rm -rf \
    /var/lib/{apt,cache,log} \
    /usr/share/{doc,man,locale} \
    /var/cache/apt \
    /home/*/.cache \
    /root/.cache \
    /var/tmp/* \
    /tmp/* \
  && usermod -aG docker ubuntu \
  && eval "${TEST}" \
  && echo

#################################################################################
# Install kubectl
#################################################################################

# SC2086: TEST variable contains a command string that needs to be evaluated, word splitting is intentional
# hadolint ignore=SC2086
RUN echo \
  && export NAME="kubectl" \
  && export TEST="${NAME} version --client" \
  && VERSION="$(/usr/bin/curl --silent --show-error --tlsv1.2 --location -L -s https://dl.k8s.io/release/stable.txt)" \
  && export VERSION \
  && ARCH="$(uname -m | awk '{ if ($1 == "x86_64") print "amd64"; else if ($1 == "aarch64" || $1 == "arm64") print "arm64"; else print "unknown" }')" \
  && export ARCH \
  && export URL="https://dl.k8s.io/release/${VERSION}/bin/linux/${ARCH}/kubectl" \
  && echo "---------------------------------------------------------"\
  && echo "INFO[${NAME}] Installed:" \
  && echo "INFO[${NAME}]   Command:        ${NAME}" \
  && echo "INFO[${NAME}]   Latest Release: ${VERSION}" \
  && echo "INFO[${NAME}]   Architecture:   ${ARCH}" \
  && echo "INFO[${NAME}]   Source:         ${URL}" \
  && echo "---------------------------------------------------------"\
  && /usr/bin/curl --silent --show-error --tlsv1.2 --location "${URL}" --output "/tmp/${NAME}" \
  && install -m 755 -o root -g root /tmp/${NAME} /usr/local/bin/${NAME} \
  && rm -rf \
  /var/lib/{apt,cache,log} \
  /usr/share/{doc,man,locale} \
  /var/cache/apt \
  /home/*/.cache \
  /root/.cache \
  /var/tmp/* \
  /tmp/* \
  && eval "${TEST}" \
  && echo

#################################################################################
# Install Helm
#################################################################################

# TODO: Install helm via mise instead of curl - GitHub API rate limiting causes failures
# RUN echo \
#   && export NAME="helm" \
#   && export TEST="${NAME} version" \
#   && export REPOSITORY="helm/helm" \
#   && export VERSION="$(/usr/bin/curl --silent --show-error --tlsv1.2 --location https://api.github.com/repos/${REPOSITORY}/releases/latest | jq --raw-output .tag_name)" \
#   && export ARCH=$(uname -m | awk '{ if ($1 == "x86_64") print "amd64"; else if ($1 == "aarch64" || $1 == "arm64") print "arm64"; else print "unknown" }') \
#   && export PKG="${NAME}-${VERSION}-linux-${ARCH}.tar.gz" \
#   && export URL="https://get.helm.sh/${PKG}" \
#   && echo "---------------------------------------------------------"\
#   && echo "INFO[${NAME}] Installed:" \
#   && echo "INFO[${NAME}]   Command:        ${NAME}" \
#   && echo "INFO[${NAME}]   Package:        ${PKG}" \
#   && echo "INFO[${NAME}]   Latest Release: ${VERSION}" \
#   && echo "INFO[${NAME}]   Architecture:   ${ARCH}" \
#   && echo "INFO[${NAME}]   Source:         ${URL}" \
#   && echo "---------------------------------------------------------"\
#   && /usr/bin/curl --silent --show-error --tlsv1.2 --location ${URL} | tar xz -C /tmp \
#   && install -m 755 -o root -g root /tmp/linux-${ARCH}/${NAME} /usr/local/bin/${NAME} \
#   && rm -rf \
#   /var/lib/{apt,cache,log} \
#   /usr/share/{doc,man,locale} \
#   /var/cache/apt \
#   /home/*/.cache \
#   /root/.cache \
#   /var/tmp/* \
#   /tmp/* \
#   && eval "${TEST}" \
#   && echo

#################################################################################
# Install GitHub Actions Runner
#################################################################################

# DL3003: Using cd instead of WORKDIR because we need to extract files in this directory within the same RUN command
# SC2086: TEST variable contains a command string that needs to be evaluated, word splitting is intentional
# hadolint ignore=DL3003,SC2086
RUN echo \
  && export NAME="actions-runner" \
  && export REPOSITORY="actions/runner" \
  && VERSION="$(/usr/bin/curl --silent --show-error --tlsv1.2 --location https://api.github.com/repos/${REPOSITORY}/releases/latest | jq --raw-output .tag_name)" \
  && export VERSION \
  && ARCH="$(uname -m | awk '{ if ($1 == "x86_64") print "x64"; else if ($1 == "aarch64" || $1 == "arm64") print "arm64"; else print "unknown" }')" \
  && export ARCH \
  && export PKG="${NAME}-linux-${ARCH}-${VERSION#v}.tar.gz" \
  && export URL="https://github.com/${REPOSITORY}/releases/download/${VERSION}/${PKG}" \
  && echo "---------------------------------------------------------"\
  && echo "INFO[${NAME}] Installed:" \
  && echo "INFO[${NAME}]   Package:        ${PKG}" \
  && echo "INFO[${NAME}]   Latest Release: ${VERSION}" \
  && echo "INFO[${NAME}]   Architecture:   ${ARCH}" \
  && echo "INFO[${NAME}]   Source:         ${URL}" \
  && echo "---------------------------------------------------------"\
  && cd /opt/actions-runner \
  && /usr/bin/curl --silent --show-error --tlsv1.2 --location "${URL}" | tar xz \
  && ./bin/installdependencies.sh \
  && chown -R ubuntu:ubuntu /opt/actions-runner \
  && echo

#################################################################################
# Install task (Taskfile) - from current Dockerfile
#################################################################################

# SC2086: TEST variable contains a command string that needs to be evaluated, word splitting is intentional
# hadolint ignore=SC2086
RUN echo \
  && export NAME="task" \
  && export TEST="${NAME} --version" \
  && export REPOSITORY="go-task/task" \
  && VERSION="$(/usr/bin/curl --silent --show-error --tlsv1.2 --location https://api.github.com/repos/${REPOSITORY}/releases/latest | jq --raw-output .tag_name)" \
  && export VERSION \
  && ARCH="$(uname -m | awk '{ if ($1 == "x86_64") print "amd64"; else if ($1 == "aarch64" || $1 == "arm64") print "arm64"; else print "unknown" }')" \
  && export ARCH \
  && export PKG="${NAME}_linux_${ARCH}.tar.gz" \
  && export URL="https://github.com/${REPOSITORY}/releases/download/${VERSION}/${PKG}" \
  && echo "---------------------------------------------------------"\
  && echo "INFO[${NAME}] Installed:" \
  && echo "INFO[${NAME}]   Command:        ${NAME}" \
  && echo "INFO[${NAME}]   Package:        ${PKG}" \
  && echo "INFO[${NAME}]   Latest Release: ${VERSION}" \
  && echo "INFO[${NAME}]   Architecture:   ${ARCH}" \
  && echo "INFO[${NAME}]   Source:         ${URL}" \
  && echo "---------------------------------------------------------"\
  && /usr/bin/curl --silent --show-error --tlsv1.2 --location "${URL}" | tar xz -C /tmp \
  && install -m 755 -o root -g root "/tmp/${NAME}" "/usr/local/bin/${NAME}" \
  && rm -rf \
  /var/lib/{apt,cache,log} \
  /usr/share/{doc,man,locale} \
  /var/cache/apt \
  /home/*/.cache \
  /root/.cache \
  /var/tmp/* \
  /tmp/* \
  && eval "${TEST}" \
  && echo

#################################################################################
# Install Hugo - from current Dockerfile
#################################################################################

# Hugo installation commented out - not needed currently
# RUN echo \
#   && export NAME="hugo" \
#   && export TEST="${NAME} version" \
#   && export REPOSITORY="gohugoio/hugo" \
#   && export VERSION="$(/usr/bin/curl --silent --show-error --tlsv1.2 --location https://api.github.com/repos/${REPOSITORY}/releases/latest | jq --raw-output .tag_name)" \
#   && export ARCH=$(uname -m | awk '{ if ($1 == "x86_64") print "amd64"; else if ($1 == "aarch64" || $1 == "arm64") print "arm64"; else print "unknown" }') \
#   && export PKG="${NAME}_extended_${VERSION#v}_linux-${ARCH}.tar.gz" \
#   && export URL="https://github.com/${REPOSITORY}/releases/download/${VERSION}/${PKG}" \
#   && echo "---------------------------------------------------------"\
#   && echo "INFO[${NAME}] Installed:" \
#   && echo "INFO[${NAME}]   Command:        ${NAME}" \
#   && echo "INFO[${NAME}]   Package:        ${PKG}" \
#   && echo "INFO[${NAME}]   Latest Release: ${VERSION}" \
#   && echo "INFO[${NAME}]   Architecture:   ${ARCH}" \
#   && echo "INFO[${NAME}]   Source:         ${URL}" \
#   && echo "---------------------------------------------------------"\
#   && /usr/bin/curl --silent --show-error --tlsv1.2 --location ${URL} | tar xz -C /tmp \
#   && install -m 755 -o root -g root /tmp/${NAME} /usr/local/bin/${NAME} \
#   && rm -rf \
#   /var/lib/{apt,cache,log} \
#   /usr/share/{doc,man,locale} \
#   /var/cache/apt \
#   /home/*/.cache \
#   /root/.cache \
#   /var/tmp/* \
#   /tmp/* \
#   && eval "${TEST}" \
#   && echo

#################################################################################
# Switch to non-root user
#################################################################################

USER ubuntu
WORKDIR /home/ubuntu

#################################################################################
# Install mise for tool version management
#################################################################################

# SC2016: Single quotes prevent expansion - we want literal strings written to shell config files
# hadolint ignore=SC2016
RUN echo \
  && curl https://mise.run | sh \
  && echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc \
  && echo 'eval "$(~/.local/bin/mise activate zsh)"' >> ~/.zshrc \
  && echo

#################################################################################
# Pre-install mise plugins for faster container startup
# Note: go, node, python, rust are core plugins and don't need installation
#################################################################################

# TODO: Investigate which plugins are actually needed vs core plugins
# RUN echo \
#   && ~/.local/bin/mise plugins install java \
#   && ~/.local/bin/mise plugins install ruby \
#   && ~/.local/bin/mise plugins install terraform \
#   && ~/.local/bin/mise plugins install kubectl \
#   && ~/.local/bin/mise plugins install helm \
#   && ~/.local/bin/mise plugins install hugo \
#   && ~/.local/bin/mise plugins install task \
#   && echo

#################################################################################
# Configure shell environment
#################################################################################

# SC2016: Single quotes prevent expansion - we want literal strings written to shell config files
# hadolint ignore=SC2016
RUN echo \
  && echo 'eval "$(direnv hook bash)"' >> ~/.bashrc \
  && echo 'eval "$(direnv hook zsh)"' >> ~/.zshrc \
  && echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc \
  && echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc \
  && echo 'export PATH="$HOME/.local/share/mise/shims:$PATH"' >> ~/.bashrc \
  && echo 'export PATH="$HOME/.local/share/mise/shims:$PATH"' >> ~/.zshrc \
  && echo 'export PATH="/workspace/.npm-global/bin:$PATH"' >> ~/.bashrc \
  && echo 'export PATH="/workspace/.npm-global/bin:$PATH"' >> ~/.zshrc \
  && echo 'export PATH="/go/bin:$PATH"' >> ~/.bashrc \
  && echo 'export PATH="/go/bin:$PATH"' >> ~/.zshrc \
  && echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc \
  && echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.zshrc \
  && echo 'export EDITOR=vim' >> ~/.bashrc \
  && echo 'export EDITOR=vim' >> ~/.zshrc \
  && echo

#################################################################################
# Setup shell prompt for better context awareness
#################################################################################

RUN printf 'export PS1="\\[\\033[01;32m\\]\\u@braincraftio\\[\\033[00m\\]:\\[\\033[01;34m\\]\\w\\[\\033[00m\\]\\$ "\n' >> ~/.bashrc

#################################################################################
# Install starship prompt for better developer experience
#################################################################################

# SC2016: Single quotes prevent expansion - we want literal strings written to shell config files
# hadolint ignore=SC2016
RUN echo \
  && /usr/bin/curl --silent --show-error --tlsv1.2 --location https://starship.rs/install.sh | sh -s -- -y -b ~/.local/bin \
  && echo 'eval "$(starship init bash)"' >> ~/.bashrc \
  && echo 'eval "$(starship init zsh)"' >> ~/.zshrc \
  && echo

#################################################################################
# Configure starship
#################################################################################

COPY <<EOF /home/ubuntu/.config/starship.toml
[container]
disabled = true

[directory]
truncation_length = 8
truncate_to_repo = true

[git_branch]
symbol = "🌱 "

[git_status]
ahead = "⇡\${count}"
behind = "⇣\${count}"
diverged = "⇡\${ahead_count}⇣\${behind_count}"
staged = "[++(\$count)](green)"

[nodejs]
symbol = "⬢ "

[python]
symbol = "🐍 "

[rust]
symbol = "🦀 "

[golang]
symbol = "🐹 "

[package]
symbol = "📦 "
EOF

#################################################################################
# Configure direnv whitelist and mise trusted paths
#################################################################################

# Create config directories
RUN mkdir -p ~/.config/direnv ~/.config/mise

# Copy configuration files
COPY --chown=ubuntu:ubuntu config/direnv.toml /home/ubuntu/.config/direnv/direnv.toml
COPY --chown=ubuntu:ubuntu config/mise.toml /home/ubuntu/.config/mise/config.toml

# Change to workspace
WORKDIR /workspace

# ============================================
# Development stage - includes all tools
# ============================================
FROM base AS development

# Install GitHub CLI from official repository (already has gh from apt, but ensure latest)
USER root
# SC3009: Brace expansion {apt,cache,log} is a bash feature, but our SHELL is set to /bin/bash so this is safe
# hadolint ignore=SC3009
RUN echo \
  && export NAME="gh" \
  && export TEST="${NAME} --version" \
  && echo "---------------------------------------------------------" \
  && echo "INFO[${NAME}] Installing GitHub CLI from official repo" \
  && echo "---------------------------------------------------------" \
  && curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
  && chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
  && apt-get update \
  && TERM=linux DEBIAN_FRONTEND=noninteractive apt-get install -q --yes --no-install-recommends --purge --assume-yes --auto-remove --allow-downgrades -o Dpkg::Options::='--force-confdef' -o Dpkg::Options::='--force-confold' gh \
  && apt-get clean && apt-get autoremove -y && apt-get purge -y --auto-remove \
  && rm -rf \
    /var/lib/{apt,cache,log} \
    /usr/share/{doc,man,locale} \
    /var/cache/apt \
    /home/*/.cache \
    /root/.cache \
    /var/tmp/* \
    /tmp/* \
  && eval "${TEST}" \
  && echo

#################################################################################
# Install act CLI
#################################################################################

# SC2086: TEST variable contains a command string that needs to be evaluated, word splitting is intentional
# hadolint ignore=SC2086
RUN echo \
  && export NAME="act" \
  && export TEST="${NAME} --version" \
  && export REPOSITORY="nektos/act" \
  && VERSION="$(/usr/bin/curl --silent --show-error --tlsv1.2 --location https://api.github.com/repos/${REPOSITORY}/releases/latest | jq --raw-output .tag_name)" \
  && export VERSION \
  && ARCH="$(uname -m | awk '{ if ($1 == "x86_64") print "x86_64"; else if ($1 == "aarch64" || $1 == "arm64") print "arm64"; else print "unknown" }')" \
  && export ARCH \
  && export PKG="${NAME}_Linux_${ARCH}.tar.gz" \
  && export URL="https://github.com/${REPOSITORY}/releases/download/${VERSION}/${PKG}" \
  && echo "---------------------------------------------------------" \
  && echo "INFO[${NAME}] Installing:" \
  && echo "INFO[${NAME}]   Command:        ${NAME}" \
  && echo "INFO[${NAME}]   Package:        ${PKG}" \
  && echo "INFO[${NAME}]   Latest Release: ${VERSION}" \
  && echo "INFO[${NAME}]   Architecture:   ${ARCH}" \
  && echo "INFO[${NAME}]   Source:         ${URL}" \
  && echo "---------------------------------------------------------" \
  && /usr/bin/curl --silent --show-error --tlsv1.2 --location "${URL}" --output "/tmp/${PKG}" \
  && tar xzf "/tmp/${PKG}" -C /usr/local/bin act \
  && chmod +x /usr/local/bin/act \
  && rm -f "/tmp/${PKG}" \
  && eval "${TEST}" \
  && echo

#################################################################################
# Switch back to ubuntu user
#################################################################################

USER ubuntu
WORKDIR /workspace

#################################################################################
# Install mise global tools that should be available immediately
#################################################################################

# Pre-install essential tools for immediate productivity
# Additional tools will be installed on first container start from workspace config
RUN echo \
  && /home/ubuntu/.local/bin/mise trust /home/ubuntu/.config/mise/config.toml \
  && /home/ubuntu/.local/bin/mise install \
  && echo

#################################################################################
# Pre-install some global npm packages for better DevX
#################################################################################

# TODO: NPM packages should be installed via workspace configuration after mise installs node
# Commenting out to prevent build failures since node isn't available yet
# RUN echo \
#   && export PATH="$HOME/.local/bin:$HOME/.local/share/mise/shims:$PATH" \
#   && ~/.local/bin/mise exec node -- npm install -g \
#   @devcontainers/cli \
#   prettier \
#   eslint \
#   typescript \
#   npm-check-updates \
#   pnpm@latest \
#   @biomejs/biome@latest \
#   playwright \
#   && echo

#################################################################################
# Install Playwright browsers
#################################################################################

# TODO: Playwright browsers should be installed after node is available via mise
# Commenting out to prevent build failures
# RUN echo \
#   && export PATH="$HOME/.local/bin:$HOME/.local/share/mise/shims:$PATH" \
#   && ~/.local/bin/mise exec node -- npx playwright install chromium firefox webkit \
#   && echo

#################################################################################
# Pre-install some Python tools
#################################################################################

# TODO: Python tools should be installed after python is available via mise
# Commenting out to prevent build failures
# RUN echo \
#   && export PATH="$HOME/.local/bin:$HOME/.local/share/mise/shims:$PATH" \
#   && ~/.local/bin/mise exec python -- pip install --user \
#   poetry \
#   pipx \
#   black \
#   ruff \
#   mypy \
#   pytest \
#   pre-commit \
#   && echo

#################################################################################
# Install Go tools
#################################################################################

# TODO: Go tools should be installed via workspace configuration
# These compile from source and significantly increase build time
# RUN echo \
#   && export GOPATH=/go \
#   && export PATH="$HOME/.local/share/mise/installs/go/latest/bin:${PATH}" \
#   && export GOLANGCI_LINT_VERSION="v1.64.8" \
#   && go install github.com/golangci/golangci-lint/cmd/golangci-lint@${GOLANGCI_LINT_VERSION} \
#   && go install golang.org/x/vuln/cmd/govulncheck@latest \
#   && go install golang.org/x/tools/gopls@latest \
#   && go install github.com/go-delve/delve/cmd/dlv@latest \
#   && go install github.com/air-verse/air@latest \
#   && echo

#################################################################################
# Install Rust toolchain and components
#################################################################################

# SC2016: Single quotes prevent expansion - we want literal strings written to shell config files
# hadolint ignore=SC2016
RUN echo \
  && /usr/bin/curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y \
  && source $HOME/.cargo/env \
  && echo 'source $HOME/.cargo/env' >> ~/.bashrc \
  && echo 'source $HOME/.cargo/env' >> ~/.zshrc \
  && echo
# TODO: Components and cargo installs should be handled by workspace configuration
# These compile from source and are the primary cause of 3+ hour builds
#   && rustup component add rustfmt clippy llvm-tools-preview rust-src rust-docs rust-analyzer \
#   && cargo install sccache \
#   && cargo install cargo-watch \
#   && cargo install cargo-edit \

#################################################################################
# Configure cargo for faster builds with lld
#################################################################################

RUN mkdir -p ~/.cargo
COPY --chown=ubuntu:ubuntu config/.cargo/config.toml /home/ubuntu/.cargo/config.toml


#################################################################################
# Configure git for container use
#################################################################################

RUN echo \
  && git config --global init.defaultBranch main \
  && git config --global pull.rebase false \
  && git config --global core.editor vim \
  && git config --global color.ui auto \
  && git config --global --add safe.directory '*' \
  && echo

#################################################################################
# Add shell completions and aliases
#################################################################################

RUN echo \
  && echo 'source <(gh completion -s bash)' >> ~/.bashrc \
  && echo 'source <(kubectl completion bash)' >> ~/.bashrc \
  && echo 'source <(mise completion bash)' >> ~/.bashrc \
  && echo "alias ll='ls -alF'" >> ~/.bashrc \
  && echo "alias la='ls -A'" >> ~/.bashrc \
  && echo "alias l='ls -CF'" >> ~/.bashrc \
  && echo "alias g='git'" >> ~/.bashrc \
  && echo "alias m='mise'" >> ~/.bashrc \
  && echo "alias mr='mise run'" >> ~/.bashrc \
  && echo "alias mt='mise tasks'" >> ~/.bashrc \
  && echo "alias dc='docker compose'" >> ~/.bashrc \
  && echo "alias k='kubectl'" >> ~/.bashrc \
  && echo

#################################################################################
# Copy entrypoint script
#################################################################################

COPY --chown=ubuntu:ubuntu bin/entrypoint /usr/local/bin/entrypoint
RUN chmod +x /usr/local/bin/entrypoint

#################################################################################
# Environment variables for development
#################################################################################

# BrainCraft.io specific environment
ENV WORKSPACE_ROOT="/workspace"
ENV GITHUB_ORG="braincraftio"
ENV PROJECT_NAME="braincraftio"

# Tool configuration
ENV DOCKER_BUILDKIT=1
ENV BUILDKIT_INLINE_CACHE=1
ENV COMPOSE_DOCKER_CLI_BUILD=1
ENV MISE_EXPERIMENTAL=1
ENV MISE_YES=1
ENV DIRENV_LOG_FORMAT=""

# Path configuration - comprehensive
ENV PATH="/home/ubuntu/.local/share/mise/shims:/home/ubuntu/.local/bin:/workspace/.npm-global/bin:/go/bin:/home/ubuntu/.cargo/bin:/home/ubuntu/.krew/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/go/bin:/nix/var/nix/profiles/default/bin"

# Language versions are managed by mise from .mise.toml
# ENV variables removed to centralize version management

# Go configuration
ENV GOPATH="/go"
ENV GOCACHE="/home/ubuntu/.cache/go-build"
ENV GOMODCACHE="/go/pkg/mod"
ENV GOPROXY="https://proxy.golang.org,direct"
ENV GONOSUMDB="*"
ENV CGO_ENABLED="1"

# Node configuration
ENV NPM_CONFIG_PREFIX="/workspace/.npm-global"
ENV PNPM_HOME="/workspace/.pnpm-store"
ENV PLAYWRIGHT_BROWSERS_PATH="/home/ubuntu/.cache/playwright"

# Cargo configuration
ENV CARGO_HOME="/home/ubuntu/.cargo"
ENV RUSTUP_HOME="/home/ubuntu/.rustup"

#################################################################################
# Labels for container metadata
#################################################################################

LABEL org.opencontainers.image.title="BrainCraft.io DevContainer"
LABEL org.opencontainers.image.description="AI-First Development Environment with Go, Rust, Node.js, Python, and GitHub Actions Runner"
LABEL org.opencontainers.image.url="https://github.com/braincraftio/workspace"
LABEL org.opencontainers.image.source="https://github.com/braincraftio/workspace"
LABEL org.opencontainers.image.vendor="BrainCraft.io"
LABEL org.opencontainers.image.authors="BrainCraft.io Community"
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL dev.containers.features="docker-outside-of-docker,github-cli"
LABEL name="BrainCraft.io Development Container"
LABEL io.k8s.display-name="BrainCraft.io Development Container"
LABEL tag="braincraftio"
LABEL distribution-scope="public"

#################################################################################
# Healthcheck
#################################################################################

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD command -v mise && command -v docker && /home/ubuntu/.cargo/bin/rustc --version || exit 1

#################################################################################
# Set the default shell to bash with login (-l) to ensure profile is loaded
#################################################################################

SHELL ["/bin/bash", "-l", "-c"]

#################################################################################
# Set entrypoint or default command
#################################################################################

# Use our entrypoint script that handles mise trust and other setup
ENTRYPOINT ["/usr/local/bin/entrypoint"]
