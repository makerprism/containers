ARG NODE_VERSION=20.20.0
ARG ALPINE_VERSION=3.22
FROM node:${NODE_VERSION}-alpine${ALPINE_VERSION}

ARG PNPM_VERSION=10.29.3
ARG DUNE_VERSION=3.21.0

RUN apk add --no-cache \
        bash \
        build-base \
        ca-certificates \
        curl \
        git \
        jq \
        pkgconf \
        postgresql16-client \
        tar \
        unzip \
        xz \
        zstd \
    && corepack enable \
    && corepack prepare "pnpm@${PNPM_VERSION}" --activate \
    && curl -fsSL https://github.com/ocaml-dune/dune-bin-install/releases/download/v3/install.sh \
      | sh -s -- "${DUNE_VERSION}" --install-root /usr/local --no-update-shell-config \
    && rm -rf /var/cache/apk/*

WORKDIR /workspace
