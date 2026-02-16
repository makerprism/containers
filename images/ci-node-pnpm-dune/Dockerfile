FROM ubuntu:24.04

ARG DEBIAN_FRONTEND=noninteractive
ARG NODE_MAJOR=20
ARG PNPM_VERSION=10.29.3
ARG DUNE_VERSION=3.21.0

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        git \
        gnupg \
        unzip \
        xz-utils \
        python3 \
        python3-pip \
        build-essential \
        pkg-config \
        jq \
    && mkdir -p /etc/apt/keyrings \
    && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key \
      | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg \
    && echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_${NODE_MAJOR}.x nodistro main" \
      > /etc/apt/sources.list.d/nodesource.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends nodejs \
    && corepack enable \
    && corepack prepare "pnpm@${PNPM_VERSION}" --activate \
    && curl -fsSL https://github.com/ocaml-dune/dune-bin-install/releases/download/v3/install.sh \
      | sh -s -- "${DUNE_VERSION}" --install-root /usr/local --no-update-shell-config \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace
