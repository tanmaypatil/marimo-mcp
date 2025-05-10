FROM mcr.microsoft.com/vscode/devcontainers/python:3.11

# Set environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONIOENCODING=UTF-8

# Install vim (other dependencies already in base image)
RUN apt-get update && apt-get install -y --no-install-recommends \
    vim \
    && rm -rf /var/lib/apt/lists/*

# Install uv package manager and ensure it's in PATH immediately
RUN curl -sSf https://astral.sh/uv/install.sh | sh && \
    ln -sf /root/.cargo/bin/uv /usr/local/bin/uv

# Update PATH for current session
ENV PATH="/root/.cargo/bin:${PATH}"

# Set working directory
WORKDIR /workspaces/app

# Configure zsh with aliases and PATH
RUN touch ~/.zshrc && \
    echo 'export PATH="/workspaces/app/.venv/bin:$PATH"' >> ~/.zshrc && \
    echo 'export PATH="/root/.cargo/bin:$PATH"' >> ~/.zshrc && \
    echo 'alias python="python3"' >> ~/.zshrc && \
    echo 'alias uvx="uv tool run"' >> ~/.zshrc

# Ensure PATH and aliases are available in all shells
RUN echo 'export PATH="/root/.cargo/bin:$PATH"' >> /etc/zsh/zshenv && \
    echo 'alias uvx="uv tool run"' >> /etc/zsh/zshenv

# Create a Python virtual environment using the full path to uv
RUN /usr/local/bin/uv venv /workspaces/app/.venv

# Update PATH to use the venv by default
ENV PATH="/workspaces/app/.venv/bin:${PATH}"