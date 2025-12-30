# Workflows Library

English | [中文](README.zh-CN.md)

A collection of reusable GitHub Actions workflows and actions for Node.js projects.

## Features

- **Setup Env Action**: Automatically setup Node.js, pnpm, and install dependencies
- **Release Workflow**: Automated release workflow with changelog generation and NPM publishing

## Usage

### Setup Env Action

Use this action to setup your Node.js environment with pnpm:

```yaml
- name: Setup Env
  uses: refinist/workflows/setup-env@v1
  with:
    node-version: 'lts/*' # Optional, defaults to 'lts/*'
```

**Inputs:**

- `node-version` (optional): Node.js version spec in SemVer notation. Defaults to `'lts/*'`. Supports aliases like `lts/*`, `latest`, `nightly`, and `canary`.

**What it does:**

1. Checks out the repository
2. Installs pnpm
3. Sets up Node.js with the specified version
4. Configures pnpm cache
5. Installs dependencies using `pnpm install`

### Release Workflow

Use this workflow to automate your release process:

```yaml
name: Release
on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    uses: refinist/workflows/.github/workflows/release.yml@v1
    with:
      changelogithub: true
      build: 'pnpm run build'
      publish: true
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

**Inputs:**

- `changelogithub` (optional): Whether to let GitHub generate the changelog. Note: GITHUB_TOKEN needs to be configured. Defaults to `true`.
- `build` (optional): The command to build the project. If your project already has `"prepublishOnly": "pnpm run build"` configured, you can skip this. Defaults to `''`.
- `publish` (optional): Whether to publish to NPM. Defaults to `true`.

**What it does:**

1. Sets up the environment using the Setup Env action
2. Generates GitHub changelog (if enabled)
3. Builds the project (if build command is provided)
4. Publishes to NPM (if enabled)

## Requirements

- Node.js (LTS recommended)
- pnpm (automatically installed by the Setup Env action)

## License

[MIT](https://opensource.org/licenses/MIT)

Copyright (c) 2025-present, Zhifeng (Jeff) Wang
