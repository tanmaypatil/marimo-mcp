FROM mcr.microsoft.com/vscode/devcontainers/python:3.11

# Set environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONIOENCODING=UTF-8

# Install vim (other dependencies already in base image)
# Install vim and ensure node/npm are up to date
RUN apt-get update && apt-get install -y --no-install-recommends \
    vim \
    nodejs \
    npm \
    && rm -rf /var/lib/apt/lists/* \
    && npm install -g npm@latest \
    && node -v && npm -v && npx -v || echo "NPX not found, installing..." && npm install -g npx
# Set working directory
WORKDIR /workspaces/app

# Configure shell settings
RUN touch ~/.zshrc && \
    echo 'alias python="python3"' >> ~/.zshrc && \
    echo 'alias uvx="uv tool run"' >> ~/.zshrc && \
    mkdir -p /etc/zsh && \
    touch /etc/zsh/zshenv && \
    echo 'alias uvx="uv tool run"' >> /etc/zsh/zshenv

# Use Python's built-in venv module to create the virtual environment
RUN python -m venv /workspaces/app/.venv

# Update PATH to use the venv by default
ENV PATH="/workspaces/app/.venv/bin:${PATH}"