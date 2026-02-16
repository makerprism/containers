# Makerprism Containers

Reusable container images for Makerprism engineering workflows.

This repository publishes versioned images to GitHub Container Registry (GHCR) for:
- GitHub Actions CI jobs across Makerprism repositories
- Backend Docker builds that need a stable toolchain base

## Published Images

### `ghcr.io/makerprism/ci-node-pnpm-dune`

Purpose: generic CI image for frontend + mixed Node/OCaml workflows.

Includes:
- Alpine Linux 3.22
- Node.js 20.20.0
- pnpm 10
- dune 3.21
- common CI tools (`git`, `curl`, `jq`, `unzip`, etc.)

Compatibility note: this image is Alpine-based (`musl`), not Ubuntu/Debian (`glibc`).

Version source: `images/ci-node-pnpm-dune/VERSION`

### `ghcr.io/makerprism/backend-builder-base`

Purpose: generic backend Docker builder base for static OCaml/PostgreSQL-linked binaries.

Includes:
- Alpine 3.20
- OCaml build deps + static libs
- dune 3.21
- PostgreSQL static client libraries built from source (v16.3)

Version source: `images/backend-builder-base/VERSION`

## Tagging Policy

Each image is published with:
- exact version tag from `VERSION` (recommended for consumers)
- major alias tag (e.g. `:1`)
- immutable commit tag (`:sha-<gitsha>`)

For production-grade consumers, pinning by digest is recommended.

## Publishing

Workflow: `.github/workflows/build-images.yml`

Triggers:
- push to `main` when image/workflow files change
- pull requests for validation builds (no publish)
- manual run (`workflow_dispatch`)

By default, images are published to GHCR with repository-scoped access.

## Visibility

Current plan: org-private packages while validating stability.

When ready, package visibility can be switched to public in GHCR package settings without changing image names.

## Why GHCR first (vs Docker Hub)

GHCR is currently preferred because:
- native GitHub Actions auth with `GITHUB_TOKEN`
- straightforward org-private package access control
- no extra Docker Hub credential handling for CI

Docker Hub mirroring can be added later as an additional publish target once images are stable.

## Consumer Examples

### GitHub Actions job container

```yaml
jobs:
  build_frontend:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/makerprism/ci-node-pnpm-dune:1.0.0
    steps:
      - uses: actions/checkout@v4
      - run: pnpm --version && dune --version
```

### Dockerfile backend builder base

```dockerfile
FROM ghcr.io/makerprism/backend-builder-base:1.0.0 AS builder
WORKDIR /app
COPY . .
RUN dune build --profile=release bin/main.exe
```
