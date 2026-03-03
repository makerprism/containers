# coding-agent-base

Base image for AI coding agents (Claude Code, OpenCode, Codex, etc.)

## Purpose

Provide a consistent, isolated environment for coding agents that can work on repositories safely.

## Includes

- Alpine Linux 3.22
- Node.js 20.20.0
- Git
- GitHub CLI (`gh`)
- Build toolchain (`build-base`)
- SSH client (for git over SSH)
- Common utilities (`curl`, `jq`, `tar`, etc.)

## Security Features

- Non-root user (`agent`) by default
- No host filesystem access (use volume mounts)
- Minimal attack surface (Alpine-based)
- No privileged operations

## Usage

### Basic run with volume mount

```bash
docker run -it --rm \
  -v /path/to/repo:/workspace \
  -e ANTHROPIC_API_KEY=your-key \
  ghcr.io/makerprism/coding-agent-base:1.0.0 \
  /bin/bash
```

### With OpenCode

```bash
docker run -it --rm \
  -v /path/to/repo:/workspace \
  -e ANTHROPIC_API_KEY=your-key \
  ghcr.io/makerprism/coding-agent-base:1.0.0 \
  npx opencode-ai
```

### With Claude Code

```bash
docker run -it --rm \
  -v /path/to/repo:/workspace \
  -e ANTHROPIC_API_KEY=your-key \
  ghcr.io/makerprism/coding-agent-base:1.0.0 \
  npx @zed-industries/claude-agent-acp
```

## Customizing

For project-specific needs, extend this image:

```dockerfile
FROM ghcr.io/makerprism/coding-agent-base:1.0.0

# Add OCaml toolchain
USER root
RUN apk add --no-cache ocaml opam
USER agent

# Install additional npm tools
RUN npm install -g some-tool
```

## Versioning

See [VERSION](./VERSION) file. Follows semantic versioning.
