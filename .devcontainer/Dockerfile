FROM mcr.microsoft.com/vscode/devcontainers/python:3.11

# Set environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    UV_SYSTEM_PYTHON=true

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    build-essential \
    zsh \
    vim \
    && rm -rf /var/lib/apt/lists/*

# Install uv package manager
RUN curl -sSf https://astral.sh/uv/install.sh | sh && \
    ln -sf /root/.cargo/bin/uv /usr/local/bin/uv

# Set working directory
WORKDIR /workspaces/app

# Install convenience utilities for GitHub Codespaces
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    chsh -s /bin/zsh && \
    echo 'export PATH="/workspaces/app/.venv/bin:$PATH"' >> ~/.zshrc && \
    echo 'alias python="python3"' >> ~/.zshrc && \
    echo 'alias uvx="uv tool run"' >> ~/.zshrc

# Create a Python virtual environment
RUN uv venv /workspaces/app/.venv

# Update PATH to use the venv by default
ENV PATH="/workspaces/app/.venv/bin:$PATH"

# Create the uvx alias globally
RUN echo 'alias uvx="uv tool run"' >> /etc/bash.bashrc