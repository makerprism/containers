ARG NODE_VERSION=20.20.0
ARG ALPINE_VERSION=3.22
FROM node:${NODE_VERSION}-alpine${ALPINE_VERSION}

ARG PNPM_VERSION=10.29.3
ARG DUNE_VERSION=3.23.0

ARG ATLAS_VERSION=latest

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
        ocaml \
        openssl-dev \
        pkgconf \
        postgresql16-client \
        tar \
        unzip \
        xz \
        zstd \
    && curl -sSfL "https://release.ariga.io/atlas/atlas-linux-amd64-${ATLAS_VERSION}" \
        -o /usr/local/bin/atlas \
    && chmod +x /usr/local/bin/atlas \
    && corepack enable \
    && corepack prepare "pnpm@${PNPM_VERSION}" --activate \
    && curl -fsSL "https://github.com/ocaml/dune/releases/download/${DUNE_VERSION}/dune-${DUNE_VERSION}.tbz" | tar xjf - \
    && cd "dune-${DUNE_VERSION}" \
    && ./configure --prefix=/usr \
    && make release \
    && make install \
    && cd .. \
    && rm -rf "dune-${DUNE_VERSION}" \
    && rm -rf /var/cache/apk/*

WORKDIR /workspace
