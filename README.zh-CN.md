# Github Workflows

[English](README.md) | 中文

一套可复用的 GitHub Actions workflows 和 actions 集合，适用于 Node.js 项目。

## 特性

- **Setup Env Action**: 自动设置 Node.js、pnpm 并安装依赖
- **Release Workflow**: 自动化发布工作流，包含 changelog 生成和 NPM 发布

## 使用方法

### Setup Env Action

使用此 action 来设置你的 Node.js 环境（使用 pnpm）：

```yaml
- name: Setup Env
  uses: refinist/workflows/setup-env@v1
  with:
    node-version: 'lts/*' # 可选，默认为 'lts/*'
```

**输入参数：**

- `node-version`（可选）：Node.js 版本规范（SemVer 格式）。默认为 `'lts/*'`。支持别名如 `lts/*`、`latest`、`nightly` 和 `canary`。

**功能说明：**

1. 检出代码仓库
2. 安装 pnpm
3. 设置指定版本的 Node.js
4. 配置 pnpm 缓存
5. 使用 `pnpm install` 安装依赖

### Release Workflow

使用此工作流来自动化你的发布流程：

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    uses: refinist/workflows/release.yml@v1
```

**输入参数：**

- `changelogithub`（可选）：是否让 GitHub 生成 changelog。注意：需要配置 GITHUB_TOKEN。默认为 `true`。
- `build`（可选）：构建项目的命令。如果你的项目已经配置了 `"prepublishOnly": "pnpm run build"`，可以跳过此参数。默认为 `''`。
- `publish`（可选）：是否发布到 NPM。默认为 `true`。

**功能说明：**

1. 使用 Setup Env action 设置环境
2. 生成 GitHub changelog（如果启用）
3. 构建项目（如果提供了构建命令）
4. 发布到 NPM（如果启用）

## 要求

- Node.js（推荐使用 LTS 版本）
- pnpm（Setup Env action 会自动安装）

## License

[MIT](https://opensource.org/licenses/MIT)

Copyright (c) 2025-present, Zhifeng (Jeff) Wang
