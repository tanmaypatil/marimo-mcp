FROM mcr.microsoft.com/vscode/devcontainers/python:3.11

# Set environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    UV_SYSTEM_PYTHON=true

# Install system dependencies - only vim needed as others are likely in the base image
RUN apt-get update && apt-get install -y --no-install-recommends \
    vim \
    && rm -rf /var/lib/apt/lists/*

# Install uv package manager
RUN curl -sSf https://astral.sh/uv/install.sh | sh && \
    ln -sf /root/.cargo/bin/uv /usr/local/bin/uv

# Set working directory
WORKDIR /workspaces/app

# Configure shell with aliases and PATH
RUN touch ~/.zshrc && \
    echo 'export PATH="/workspaces/app/.venv/bin:$PATH"' >> ~/.zshrc && \
    echo 'alias python="python3"' >> ~/.zshrc && \
    echo 'alias uvx="uv tool run"' >> ~/.zshrc

# Create the uvx alias globally for all shells
RUN echo 'alias uvx="uv tool run"' >> /etc/bash.bashrc

# Create a Python virtual environment
RUN uv venv /workspaces/app/.venv

# Update PATH to use the venv by default
ENV PATH="/workspaces/app/.venv/bin:$PATH"