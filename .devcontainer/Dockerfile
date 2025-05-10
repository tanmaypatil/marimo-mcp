FROM mcr.microsoft.com/vscode/devcontainers/python:3.11

# Set environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONIOENCODING=UTF-8

# Install vim (other dependencies already in base image)
RUN apt-get update && apt-get install -y --no-install-recommends \
    vim \
    && rm -rf /var/lib/apt/lists/*

# Install uv package manager without relying on PATH updates
RUN curl -sSf https://astral.sh/uv/install.sh | sh 

# Create aliases and set up PATH for interactive shells
RUN touch ~/.zshrc && \
    echo 'export PATH="/root/.cargo/bin:$PATH"' >> ~/.zshrc && \
    echo 'export PATH="/workspaces/app/.venv/bin:$PATH"' >> ~/.zshrc && \
    echo 'alias python="python3"' >> ~/.zshrc && \
    echo 'alias uvx="uv tool run"' >> ~/.zshrc && \
    mkdir -p /etc/zsh && \
    touch /etc/zsh/zshenv && \
    echo 'export PATH="/root/.cargo/bin:$PATH"' >> /etc/zsh/zshenv && \
    echo 'alias uvx="uv tool run"' >> /etc/zsh/zshenv

# Set working directory
WORKDIR /workspaces/app

# Create a Python virtual environment using the absolute path to uv
RUN /root/.cargo/bin/uv venv /workspaces/app/.venv

# Update PATH to use the venv by default
ENV PATH="/workspaces/app/.venv/bin:/root/.cargo/bin:${PATH}"