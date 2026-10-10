---
title: "Linux Container Development"
description: "A practical workflow for building repeatable development environments with containers."
date: 2026-10-10
updated: 2026-10-10
category: "linux"
tags:
  - linux
  - containers
  - development
published: true
projects:
  - notes
---

# Linux Container Development

Containers make development environments repeatable by packaging a project with its runtime, system libraries, and tools. A good development container should preserve fast feedback while keeping the host clean.

## A minimal workflow

1. Choose a base image that matches the project runtime.
2. Install only the tools required to build and test.
3. Mount the source tree into the container during development.
4. Persist dependency caches in a named volume when useful.
5. Run tests and formatters inside the container so local results match CI.

Example commands:

```bash
docker build -t notes-dev .
docker run --rm -it \
  -v "$PWD:/workspace" \
  -w /workspace \
  notes-dev
```

## Practical considerations

- Use a non-root user for day-to-day development when possible.
- Keep secrets out of images and pass them at runtime through your secret mechanism.
- Add a `.dockerignore` file to avoid copying build output, dependencies, and credentials.
- Use multi-stage builds for production images so compilers and package managers are left behind.
- Pin important image versions and rebuild regularly for security updates.

The container is a boundary, not a substitute for source control or reproducible dependency files. Keep the setup documented and make the common development command easy to discover.
