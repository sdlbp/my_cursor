---
name: oss-readme-generator
description: Generate bilingual (English and Chinese) README files for open source tools with a standard structure. Use when creating or updating README documents for open source repositories, especially when the user mentions README, documentation, or /docs/README-zh.md.
---

# OSS README Generator

## Instructions

当用户需要为**开源工具 / 代码库**生成或更新 README 时，使用本 Skill。  
目标是生成**中英文双语**的 README：
- **英文版**：作为默认主 README 内容
- **中文版**：输出到 `/docs/README-zh.md`（内容为中文）

你需要根据代码库的实际情况，**智能调整和填充**下面的结构：可以增加或删除章节，但默认应包含以下核心部分。

### 1. 信息收集

1. 从代码库中收集信息（可以结合用户描述）：
   - 代码库 / 工具名称
   - 核心功能点、适用场景
   - 安装方式、依赖（语言运行时、包管理器、系统依赖等）
   - 基本使用示例（命令行、代码片段、配置示例等）
   - 对外暴露的 API（函数、CLI 命令、HTTP 接口等）
   - License 类型
2. 如果信息缺失：
   - 尽量通过代码、`package.json`、`pyproject.toml`、`Cargo.toml`、`go.mod` 等文件推断
   - 无法确定时，用简短占位语句，并**标注为 TODO**，避免编造具体技术细节。

### 2. 英文 README 结构（主 README）

生成英文 README 的默认结构如下，可根据实际情况适当增删小节，但不要改变章节的**大体顺序**：

```markdown
[简体中文](./docs/README-zh.md)

## What is [Project Name or Tool Name]

One concise sentence that accurately describes what this project/tool does and its core purpose.

## Features

- [Feature 1]: short explanation
- [Feature 2]: short explanation
- [Feature 3]: short explanation

## Installation

Describe dependencies first (runtime, language version, system requirements, external services), then installation steps.

### Prerequisites
- [Runtime or language version]
- [Package manager or build tool]
- [Other dependencies if any]

### Install
```bash
[# concrete installation commands, e.g. npm/pip/brew/docker, etc.]
```

## Usage

Explain how to use the tool with one or more **clear, copy-pastable examples**.

### Basic example
```bash
[# basic CLI or code usage example]
```

### Advanced example (optional)
```bash
[# optional advanced usage, configuration, or integration example]
```

## API

Document the public API in a **table**. Adjust columns to match the project type (CLI / library / HTTP API).

### Example table for a library

| Name | Type | Description | Parameters | Returns |
|------|------|-------------|------------|---------|
| `foo()` | function | [What it does] | `bar: string` | `Promise<Result>` |

### Example table for CLI commands

| Command | Arguments | Description | Example |
|---------|-----------|-------------|---------|
| `tool run` | `--config <path>` | Run the tool with config file | `tool run --config ./config.toml` |

## License

State the license clearly, for example:

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.
```

在生成英文 README 时：
- 保持语言**简洁、专业、准确**，避免营销式夸张
- 尽量使用**短句**和**项目相关的具体术语**
- 确保所有代码示例都能独立复制运行（如需上下文，在 README 中说明前置条件）

### 3. 中文 README 结构（`/docs/README-zh.md`）

中文 README 结构与英文基本对应，但用**自然中文表达**，不是逐字直译。可以根据中文读者习惯稍作调整标题用语，例如：

```markdown
## 这是什么项目（What is [项目名称]）

用一句话准确概述这个项目/工具的核心用途和价值。

## 功能特性

- [功能点 1]：简要说明
- [功能点 2]：简要说明
- [功能点 3]：简要说明

## 安装说明

先说明依赖环境，再说明具体安装步骤。

### 环境依赖
- [运行时或语言版本]
- [包管理器或构建工具]
- [其他依赖（如数据库、外部服务）]

### 安装步骤
```bash
[# 安装命令示例]
```

## 使用方法

通过示例展示如何使用。

### 基本用法
```bash
[# 基本使用示例（命令行或代码）]
```

### 进阶用法
```bash
[# 进阶配置或多场景示例]
```

## API 说明

使用表格说明 API，用中文解释参数和行为。

| 名称 | 类型 / 命令 | 说明 | 参数 | 返回值 / 输出 |
|------|-------------|------|------|----------------|
| `foo()` | 函数 | [作用说明] | `bar: string` | `Promise<Result>` |

## 许可证

说明使用的开源协议，例如：

本项目基于 MIT 协议开源，详情见 [LICENSE](../LICENSE) 文件。
```

生成中文 README 时：
- 用**地道中文**表达，适当解释专业术语
- 不强行逐字翻译英文，可根据上下文调整语序和例子

### 4. 输出要求与文件位置

当用户请求生成 README 时，**直接在对应文件位置生成内容**（而不是要求用户复制粘贴）：

1. **英文内容**：
   - 写入项目根目录的 `README.md`（若文件已存在则更新/覆盖相关内容）
   - 若生成了中文 README，需在英文 README 顶部合适位置加入中文索引链接（如 `简体中文`）
2. **中文内容**：
   - 写入 `/docs/README-zh.md`（若目录不存在先创建）
   - 保证与英文版本在信息量上大致对应

如果用户只要求生成某一语言，则仅生成对应文件。

### 5. 何时主动使用此 Skill

在以下场景，你应主动按照本 Skill 的规范来生成 README 草稿：

- 用户提到“README”、“文档”、“开源工具介绍”、“给这个仓库写一个 README”等需求
- 用户为一个新的开源项目初始化说明文档
- 用户希望把项目说明**中英双语**化，尤其是提到 `/docs/README-zh.md`

若用户请求中已经指定部分结构或章节顺序，则：
- **优先遵循用户指定的结构**
- 在不冲突的前提下，补充本 Skill 中建议的关键章节（特别是：简介、Features、Installation、Usage、API、License）

## Examples

### Example 1：最简命令行工具

用户：  
> 帮我为这个命令行工具生成 README，同时生成英文和中文版本，中文放在 /docs/README-zh.md。

你应该：
1. 分析代码找到主要命令和参数
2. 按“简介 → Features → Installation → Usage → API → License”的顺序生成英文 README
3. 生成对应的中文 README，写法自然流畅
4. 用 “English README” / “中文 README” 两个大段分别输出，方便用户复制到 `README.md` 和 `/docs/README-zh.md`。

