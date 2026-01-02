[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# workflow-github-actions

Reusable GitHub Actions workflows for tagging and releasing projects. Provides a standardised CI/CD workflow that uses Make targets for validation, enabling consistent build processes across repositories.

## Features

- Reusable workflow for consistent CI/CD
- Make-based initialisation and validation
- Automatic semantic versioning with `action-tag`
- GitHub release creation with `action-release`

## Quick Start

```yaml
name: Tag and Release

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  publish:
    permissions:
      contents: write
    uses: martoc/workflow-github-actions/.github/workflows/publish.yml@v0
```

## Prerequisites

Your repository must have a `Makefile` with `init` and `validate` targets:

```makefile
.PHONY: init validate

init:
	pip install yamllint

validate:
	yamllint *.yml
```

## Documentation

- [Usage Guide](./docs/USAGE.md) - Detailed usage instructions and examples
- [Code Style](./docs/CODESTYLE.md) - Code style guidelines for contributors

## Available Workflows

| Workflow | Description |
|----------|-------------|
| `publish.yml` | Tag and release workflow with Make-based validation |

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
