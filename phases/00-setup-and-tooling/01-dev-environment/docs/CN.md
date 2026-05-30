# 开发环境

> 你的工具会塑造你的思考方式。一次性搭好，并且搭对。

**类型：** 构建
**语言：** Python、Node.js、Rust
**前置要求：** 无
**耗时：** 约 45 分钟

## 学习目标

- 从零开始安装 Python 3.11+、Node.js 20+ 和 Rust 工具链
- 配置虚拟环境和包管理器，支持可复现的构建
- 通过 CUDA/MPS 验证 GPU 访问，并运行一个测试张量操作
- 理解四层技术栈：系统、包、运行时、AI 库

## 问题

你即将通过 200 多节课程学习 AI 工程，使用 Python、TypeScript、Rust 和 Julia。如果你的环境有问题，那么每一节课都会变成和工具链搏斗，而不是专注学习。

大多数人会跳过环境配置。然后他们会花数小时调试导入错误、版本冲突和缺失的 CUDA 驱动。我们要认真地、一次性把这件事做好。

## 核心概念

AI 工程环境有四层：

```mermaid
graph TD
    A["4. AI/ML 库\nPyTorch、JAX、transformers 等"] --> B["3. 语言运行时\nPython 3.11+、Node 20+、Rust、Julia"]
    B --> C["2. 包管理器\nuv、pnpm、cargo、juliaup"]
    C --> D["1. 系统基础\n操作系统、shell、git、编辑器、GPU 驱动"]
```

我们会自底向上安装。每一层都依赖它下面的那一层。

## 开始构建

### 第 1 步：系统基础

检查你的系统，并安装基础工具。

```bash
# macOS
xcode-select --install
brew install git curl wget

# Ubuntu/Debian
sudo apt update && sudo apt install -y build-essential git curl wget

# Windows（使用 WSL2）
wsl --install -d Ubuntu-24.04
```

### 第 2 步：使用 uv 配置 Python

我们使用 `uv`，它比 pip 快 10 到 100 倍，并且会自动处理虚拟环境。

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh

uv python install 3.12

uv venv
source .venv/bin/activate  # 或者在 Windows 上使用 .venv\Scripts\activate

uv pip install numpy matplotlib jupyter
```

验证：

```python
import sys
print(f"Python {sys.version}")

import numpy as np
print(f"NumPy {np.__version__}")
a = np.array([1, 2, 3])
print(f"Vector: {a}, dot product with itself: {np.dot(a, a)}")
```

### 第 3 步：使用 pnpm 配置 Node.js

用于 TypeScript 课程（智能体、MCP 服务器、Web 应用）。

```bash
curl -fsSL https://fnm.vercel.app/install | bash
fnm install 22
fnm use 22

npm install -g pnpm

node -e "console.log('Node', process.version)"
```

### 第 4 步：Rust

用于性能关键的课程（推理、系统）。

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

rustc --version
cargo --version
```

### 第 5 步：Julia（可选）

用于 Julia 表现出色的数学密集型课程。

```bash
curl -fsSL https://install.julialang.org | sh

julia -e 'println("Julia ", VERSION)'
```

### 第 6 步：GPU 配置（如果你有 GPU）

```bash
# NVIDIA
nvidia-smi

# 安装带 CUDA 的 PyTorch
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```

```python
import torch
print(f"CUDA available: {torch.cuda.is_available()}")
if torch.cuda.is_available():
    print(f"GPU: {torch.cuda.get_device_name(0)}")
```

没有 GPU？没关系。大多数课程都可以在 CPU 上运行。对于训练量较大的课程，可以使用 Google Colab 或云端 GPU。

### 第 7 步：验证所有配置

运行验证脚本：

```bash
python phases/00-setup-and-tooling/01-dev-environment/code/verify.py
```

## 使用它

你的环境现在已经可以支持本课程中的所有课程。下面是各语言的使用场景：

| 语言 | 使用范围 | 包管理器 |
|----------|---------|-----------------|
| Python | 第 1-12 阶段（机器学习、深度学习、NLP、视觉、音频、LLM） | uv |
| TypeScript | 第 13-17 阶段（工具、智能体、群体智能、基础设施） | pnpm |
| Rust | 第 12、15-17 阶段（性能关键系统） | cargo |
| Julia | 第 1 阶段（数学基础） | Pkg |

## 交付成果

本课会产出一个验证脚本，任何人都可以运行它来检查自己的环境配置。

请查看 `outputs/prompt-env-check.md`，其中有一个提示词，可以帮助 AI 助手诊断环境问题。

## 练习

1. 运行验证脚本，并修复所有失败项
2. 为本课程创建一个 Python 虚拟环境，并安装 PyTorch
3. 分别用四种语言编写一个 "hello world" 并运行
