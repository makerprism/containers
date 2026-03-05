# coding-agent-base
# Base image for AI coding agents (Claude Code, OpenCode, Codex, etc.)
#
# Purpose: Provide a consistent, isolated environment for coding agents
# that can be used to work on repositories safely.
#
# Features:
# - Node.js 20 + pnpm for TypeScript/JavaScript
# - Dune 3.21 with package management for OCaml
# - PostgreSQL client libraries for backend work
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
ARG DUNE_VERSION=3.21.0
FROM node:${NODE_VERSION}-alpine${ALPINE_VERSION}

# Install core tools + OCaml build dependencies (aligned with backend-builder-base)
RUN apk add --no-cache \
        bash \
        build-base \
        ca-certificates \
        curl \
        git \
        gmp-dev \
        jq \
        libev-dev \
        libffi-dev \
        libpq-dev \
        m4 \
        openssl-dev \
        openssl-libs-static \
        pkgconf \
        postgresql-dev \
        postgresql16-client \
        perl \
        readline-dev \
        readline-static \
        tar \
        unzip \
        xz \
        zstd \
        zlib-dev \
        zlib-static \
        musl-dev \
        argon2-dev \
        openssh-client \
    && rm -rf /var/cache/apk/*

# Install GitHub CLI
RUN apk add --no-cache github-cli

# Install dune (OCaml build system with package management)
RUN curl -fsSL https://github.com/ocaml-dune/dune-bin-install/releases/download/v3/install.sh \
      | sh -s -- "${DUNE_VERSION}" --install-root /usr --no-update-shell-config

# Set up workspace
WORKDIR /workspace

# Default to bash for interactive use
CMD ["/bin/bash"]
