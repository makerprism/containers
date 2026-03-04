# coding-agent-base
# Base image for AI coding agents (Claude Code, OpenCode, Codex, etc.)
#
# Purpose: Provide a consistent, isolated environment for coding agents
# that can be used to work on repositories safely.
#
# Security features:
# - No host filesystem access (use volume mounts)
# - Minimal attack surface (Alpine-based)
# - No privileged operations
# - Network access configurable at runtime
#
# Note: Runs as root for CI volume mount compatibility. The container is
# isolated by Docker, so this is safe for CI workloads.

ARG NODE_VERSION=20.20.0
ARG ALPINE_VERSION=3.22
FROM node:${NODE_VERSION}-alpine${ALPINE_VERSION}

# Install core tools + OCaml build dependencies
RUN apk add --no-cache \
        bash \
        build-base \
        ca-certificates \
        curl \
        git \
        jq \
        openssh-client \
        pkgconf \
        tar \
        unzip \
        xz \
        zstd \
        libpq-dev \
        libev-dev \
        postgresql-dev \
        gmp-dev \
        openssl-dev \
        argon2-dev \
    && rm -rf /var/cache/apk/*

# Install GitHub CLI
RUN apk add --no-cache github-cli

# Set up workspace
WORKDIR /workspace

# Default to bash for interactive use
CMD ["/bin/bash"]
