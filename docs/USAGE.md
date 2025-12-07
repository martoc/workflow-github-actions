# Usage

## Overview

This repository provides reusable GitHub Actions workflows for tagging and releasing projects.
The `deploy.yml` workflow uses Make targets for validation, providing a standardised approach
to CI/CD across repositories.

## Available Workflows

### deploy.yml (Reusable Workflow)

A reusable workflow that can be called from other repositories. It performs the following steps:

1. **Checkout** - Clones the repository with fetch depth of 10 and fetches tags
2. **Validate** - Runs `make validate` to execute project-specific validation
3. **Tag** - Creates version tags using [martoc/action-tag](https://github.com/martoc/action-tag)
4. **Release** - Creates GitHub releases using [martoc/action-release](https://github.com/martoc/action-release)

## Prerequisites

### Makefile Requirements

Your repository must have a `Makefile` with a `validate` target. This target should contain
your project-specific validation logic (linting, testing, etc.).

Example `Makefile`:

```makefile
.PHONY: validate
validate:
	yamllint *.yml
	# Add other validation commands as needed
```

### Repository Permissions

The workflow requires `contents: write` permission to create tags and releases.

## Example

To use the `deploy.yml` workflow in your repository, create a workflow file that calls it:

```yaml
---
name: Tag and Release

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  deploy:
    uses: martoc/workflow-github-actions/.github/workflows/deploy.yml@main
    permissions:
      contents: write
```

## Migration from Previous Workflows

This version introduces a breaking change from the previous workflow structure.
The following workflows have been removed:

* `action.yml`
* `default.yml`
* `github-actions.yml`
* `tag.yml`
* `workflow.yml`

These have been replaced with the single `deploy.yml` reusable workflow.

### Key Changes

1. **Make-based validation** - Validation is now performed via `make validate` instead of
   inline commands like `yamllint action.yml`
2. **Single workflow** - All previous workflows consolidated into `deploy.yml`
3. **Updated checkout action** - Uses `actions/checkout@v6`

### Migration Steps

1. Ensure your repository has a `Makefile` with a `validate` target
2. Update your workflow to call `deploy.yml` instead of the previous workflows
3. Move any inline validation commands to the `validate` target in your `Makefile`
