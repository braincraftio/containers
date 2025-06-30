# BrainCraft.io DevContainer

AI-First Development Environment with Go, Rust, Node.js, Python, and Cloud Tools.

## 🚀 Quick Start

### Build the container:
```bash
docker build --progress plain --tag ghcr.io/braincraftio/devcontainer:latest -f containers/devcontainer/Dockerfile .
```

### Run the container:
```bash
docker run --rm -d --name braincraftio --hostname braincraftio ghcr.io/braincraftio/devcontainer:latest
```

### Use with VS Code:
The devcontainer is designed to work seamlessly with VS Code's Remote-Containers extension. Simply open the workspace folder and VS Code will prompt to reopen in the container.

## 📦 Included Tools

### Languages & Runtimes
- **Go 1.23** - With golangci-lint, gopls, delve debugger
- **Node.js 24** - With pnpm, TypeScript, ESLint, Prettier
- **Python 3.13** - With Poetry, Ruff, Black, mypy
- **Rust stable** - With rustfmt, clippy, rust-analyzer

### Cloud & DevOps Tools
- **Docker CLI** - With compose and buildx plugins
- **kubectl** - Latest stable version
- **Helm** - Latest version
- **GitHub CLI (gh)** - For GitHub operations
- **act** - Run GitHub Actions locally

### Development Tools
- **mise** - Universal tool version manager
- **direnv** - Directory-specific environment variables
- **tmux** - Terminal multiplexer
- **neovim** - Modern vim
- **fzf** - Fuzzy finder
- **ripgrep** - Fast grep alternative
- **bat** - cat with syntax highlighting
- **eza** - Modern ls replacement
- **httpie** - User-friendly HTTP client

### Performance Tools
- **lld** - Fast linker for Rust
- **valgrind** - Memory debugging
- **heaptrack** - Heap memory profiler

## 🏗️ Architecture

The devcontainer follows the ContainerCraft style guide and includes:

- **Non-root user**: Runs as `ubuntu` user with sudo access
- **Persistent volumes**: Designed for volume mounts to preserve caches
- **GitHub Actions Runner**: Pre-installed and ready to configure
- **Shell completions**: For all major tools
- **Optimized paths**: All tools accessible in PATH

## 🔧 Configuration

### Environment Variables
Key environment variables are pre-configured:
- `WORKSPACE_ROOT=/workspace`
- `DOCKER_BUILDKIT=1`
- `MISE_EXPERIMENTAL=1`
- Language-specific optimizations for Go, Node, Python, and Rust

### Volume Mounts
Recommended volume mounts for performance:
```yaml
volumes:
  - workspace:/workspace
  - gomodcache:/go/pkg/mod
  - gocache:/home/ubuntu/.cache/go-build
  - npm-global:/workspace/.npm-global
  - pnpm-store:/workspace/.pnpm-store
  - cargo-registry:/home/ubuntu/.cargo/registry
  - mise-cache:/home/ubuntu/.local/share/mise
```

## 🛡️ Security

- Runs as non-root user
- Minimal attack surface
- Regular security updates
- No hardcoded secrets

## 🤝 Contributing

This devcontainer is part of the BrainCraft.io workspace. Contributions welcome!

## 📄 License

Apache 2.0 - See LICENSE for details.