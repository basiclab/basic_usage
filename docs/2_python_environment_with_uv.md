# 🚀 Using uv for Python Package Management

> [!NOTE]
> This document may be updated as better alternatives to `uv` emerge.

`uv` is a modern Python package installer and resolver, written in **Rust**. It's designed to be a faster alternative to pip and other Python package managers.

## ✨ Key Features

- ⚡ **Speed**: `uv` is significantly faster than traditional package managers
- 🔄 **Automatic Python Management**: Downloads and manages Python versions automatically
- 🔌 **Compatibility**: Works with existing Python installations
- 🎨 **Modern Interface**: Simple and intuitive command-line interface

## 📥 Installation

To install `uv`, you can use the following command:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 🎯 Basic Usage

### 🐍 Installing Python Versions

`uv` can automatically manage Python versions for you. Here are some common commands:

1. Install the latest Python version:

```bash
uv python install
```

2. Install a specific Python version:

```bash
uv python install 3.12
```

3. Install multiple Python versions:

```bash
uv python install 3.11 3.12
```

### 📦 Managing Packages

1. Initialize a new project:

```bash
uv init project_name

# or in current directory
uv init

# with python version (recommended)
uv init --python 3.12
```

2. Add dependencies to your project:

```bash
uv add package_name

# or with specific version
uv add 'package_name==1.2.3'
```

3. Add dependencies from requirements.txt:

```bash
uv add -r requirements.txt
```

4. Remove a package:

```bash
uv remove package_name
```

5. Upgrade a specific package:

```bash
uv lock --upgrade-package package_name
```

### 📁 Project Structure

A typical uv project structure looks like this:

```plaintext
.
├── .venv/              # Virtual environment
├── .python-version     # Project's Python version
├── README.md
├── main.py
├── pyproject.toml      # Project metadata and dependencies
└── uv.lock            # Lock file for reproducible installations
```

<details>
<summary>📄 Project Files Explained (Expand for details)</summary>

The `pyproject.toml` file contains metadata about your project and its dependencies. It's the main configuration file where you specify:

- Project name, version, and description
- Project dependencies
- Build system requirements
- uv-specific configuration options in the `[tool.uv]` section

Example:

```toml
[project]
name = "your-project"
version = "0.1.0"
description = "Your project description"
dependencies = []
```

#### .python-version

The `.python-version` file specifies the project's default Python version. This file tells uv which Python version to use when creating the project's virtual environment. It's automatically created when you initialize a project with a specific Python version.

#### .venv

The `.venv` directory contains your project's virtual environment. This is an isolated Python environment where uv installs all your project's dependencies. The environment is automatically created when you run your first project command (like `uv run`, `uv sync`, or `uv lock`).

#### uv.lock

The `uv.lock` file is a cross-platform lockfile that contains exact information about your project's dependencies. Unlike `pyproject.toml` which specifies broad requirements, the lockfile contains the exact resolved versions of all dependencies. This file should be committed to version control to ensure consistent and reproducible installations across different machines. The lockfile is managed by uv and should not be edited manually.
</details>

### 🌍 Running Scripts

1. Running a script starting with `uv`:

```bash
uv run script.py
```

or

2. You can choose first activate the virtual environment and run the script:

```bash
source .venv/bin/activate  # On Unix/macOS
# or
.venv\Scripts\activate     # On Windows

# then
python main.py
```

## 🔥 Using PyTorch with uv

PyTorch has some unique packaging characteristics that require special configuration when using uv:

1. PyTorch wheels are hosted on dedicated indices rather than PyPI
2. Different builds are available for different accelerators (CPU, CUDA, ROCm, etc.)
3. Builds for different accelerators are published to different indices

### Basic PyTorch Installation

To install PyTorch with uv, you'll need to configure your `pyproject.toml` to use the appropriate PyTorch index. Here's an example configuration for CPU-only builds:

```toml
[project]
name = "your-project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "torch>=2.7.0",
    "torchvision>=0.22.0",
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv.sources]
torch = [
    { index = "pytorch-cpu" },
]
torchvision = [
    { index = "pytorch-cpu" },
]
```

>[!NOTE]
> The pytorch index is the index url provide for pip and can be found from [here](https://pytorch.org/get-started/locally/)

### Configuring Different Accelerators

You can configure different PyTorch builds based on your needs:

#### CUDA Support

For CUDA support, use the appropriate CUDA index:

```toml
[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
torch = [
    { index = "pytorch-cu121", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]
torchvision = [
    { index = "pytorch-cu121", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]
```

### Quick Installation with uv pip

You can also use the `uv pip` interface for quick PyTorch installations:

```bash
# CPU-only installation
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu

# CUDA installation
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

## 📚 Additional Resources

For more detailed information, visit the official documentation at [https://docs.astral.sh/uv/](https://docs.astral.sh/uv/)
