# dev-base
# Unified development image for CI, coding agents, and local development
#
# Features:
# - Node.js 20 + pnpm for TypeScript/JavaScript
# - Dune 3.21 with package management for OCaml
# - PostgreSQL client libraries for backend work
# - Atlas for database migrations
# - GitHub CLI for PRs and issues
# - SSH client for private repos
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

# Redeclare ARGs after FROM to use in build stage
ARG PNPM_VERSION=10.29.3
ARG DUNE_VERSION=3.21.0
ARG ATLAS_VERSION=latest

# Install all dependencies: build tools, DB clients, dev utilities
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
        github-cli \
    && rm -rf /var/cache/apk/*

# Install Atlas (database migrations)
RUN curl -sSfL "https://release.ariga.io/atlas/atlas-linux-amd64-${ATLAS_VERSION}" \
        -o /usr/local/bin/atlas \
    && chmod +x /usr/local/bin/atlas

# Enable pnpm via corepack
RUN corepack enable \
    && corepack prepare "pnpm@${PNPM_VERSION}" --activate

# Install dune (OCaml build system with package management)
RUN curl -fsSL https://github.com/ocaml-dune/dune-bin-install/releases/download/v3/install.sh \
      | sh -s -- "${DUNE_VERSION}" --install-root /usr --no-update-shell-config

# Set up workspace
WORKDIR /workspace

# Default to bash for interactive use
CMD ["/bin/bash"]
