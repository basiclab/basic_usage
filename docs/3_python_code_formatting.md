# ✨ Python Code Formatting with Black and isort

This guide explains how to use Black and isort to automatically format your Python code, ensuring consistency and readability across your project.

>[!NOTE]
> If you choose using Ruff, you can skip the Black and isort installation and configuration and jump to the [Advanced Usage with Ruff](#-advanced-usage-with-ruff) section.

## 🎯 What are Black and isort?

- **Black**: An opinionated Python code formatter that automatically formats your code to comply with PEP 8 style guide
- **isort**: A Python utility that sorts your imports alphabetically and separates them into sections

> ⚠️ **Important**: Black and isort can conflict with each other's formatting. Always configure isort to be compatible with Black's formatting style.

## 📦 Installation

```bash
# Install both tools using pip
pip install black isort

# or manage it with uv, using the --dev flag to add it to the dev dependencies
uv add black isort --dev
```

## 🚀 Basic Usage

### Using Black

```bash
# Format a single file
black your_file.py

# Format all Python files in a directory
black your_directory/

# Format specific files
black file1.py file2.py

# Check what would be formatted without making changes
black --check your_file.py

# Or you can make it scan across all files in the current directory
black .
```

### Using isort

```bash
# Sort imports in a single file
isort your_file.py

# Sort imports in all Python files in a directory
isort your_directory/

# Sort imports in specific files
isort file1.py file2.py

# Check what would be sorted without making changes
isort --check-only your_file.py
```

## ⚙️ Configuration

### Black Configuration

Create a `pyproject.toml` file in your project root (`uv` will automatically create it for you):

```toml
[tool.black]
line-length = 88
target-version = ['py38']
include = '\.pyi?$'
exclude = '''
/(
    \.git
  | \.hg
  | \.mypy_cache
  | \.tox
  | \.venv
  | _build
  | buck-out
  | build
  | dist
)/
'''
```

### isort Configuration

Add to your `pyproject.toml`:

```toml
[tool.isort]
profile = "black"
multi_line_output = 3
line_length = 88
```

> We set `profile = "black"` to ensure isort is compatible with Black's formatting style.

## 💻 Integration with IDEs

### VS Code

1. Install the Python extension for [black](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter) and [isort](https://marketplace.visualstudio.com/items?itemName=ms-python.isort)
2. Add to your `settings.json`:

```json
{
    "[python]": {
        "editor.formatOnSave": true,
        "editor.codeActionsOnSave": {
            "source.organizeImports": true
        },
        "editor.defaultFormatter": "black"
    }
}
```

### PyCharm

1. Install the BlackConnect plugin
2. Install the isort plugin
3. Configure both in Settings → Tools

## 📝 Common Use Cases

### Before Formatting

```python
from datetime import datetime
import os
import sys
from typing import List,Dict
def my_function(  x: int,y: int  )->int:
    return x+y
```

### After Formatting with Black and isort

```python
import os
import sys
from datetime import datetime
from typing import Dict, List


def my_function(x: int, y: int) -> int:
    return x + y
```

## 💡 Best Practices

1. **Consistent Configuration**: Use the same configuration across your team
2. **Pre-commit Hooks**: Set up pre-commit hooks to automatically format code before commits
3. **CI/CD Integration**: Add formatting checks to your CI/CD pipeline
4. **Documentation**: Document your formatting rules in your project's README

## ⚠️ Troubleshooting

### Common Issues

1. **Black and isort conflicts**:
   - Use isort's "black" profile to ensure compatibility
   - Run isort after black

2. **Line length conflicts**:
   - Set the same line length in both tools' configurations
   - Black's default is 88 characters

3. **Import sorting issues**:
   - Use isort's `--profile black` option
   - Configure import sections in isort settings

## 🔍 Advanced Usage with Ruff

Ruff is a fast Python linter and formatter written in Rust. It can replace multiple tools like Black, isort, and various linters with a single, fast tool.

### Benefits of Using Ruff

1. **Speed**: Ruff is significantly faster than Black and isort
2. **Unified Tool**: Combines formatting and linting in one tool
3. **Compatibility**: Maintains compatibility with Black's formatting style
4. **Extensible**: Supports over 800 lint rules across 50+ built-in plugins
5. **Modern**: Written in Rust for better performance and reliability

### Installation

```bash
# Install Ruff using pip
pip install ruff

# Or manage it with uv
uv add ruff --dev
```

### Basic Usage

```bash
# Run the linter
ruff check .

# Run the formatter
ruff format .

# Fix auto-fixable issues
ruff check --fix .

# Format and fix in one command
ruff check --fix . && ruff format .
```

### Configuration

Create a `pyproject.toml` file in your project root:

```toml
[tool.ruff]
# Set the maximum line length to 88 (same as Black)
line-length = 88

# Enable all rules by default
select = ["ALL"]

# Exclude specific rules
ignore = [
    "E501",  # Line too long
    "D203",  # 1 blank line required before class docstring
    "D212",  # Multi-line docstring summary should start at the first line
]

# Target Python 3.8+. Feel free to change it to the latest version you are using.
target-version = "py38"

[tool.ruff.format]
# Use double quotes for strings
quote-style = "double"

# Indent with spaces, rather than tabs
indent-style = "space"

# Respect magic trailing commas
skip-magic-trailing-comma = false

# Like Black, respect magic trailing commas
line-ending = "auto"
```

### Integration with IDEs

#### Setup in VS Code

1. Install the [Ruff extension](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff)
2. Add to your `settings.json`:

```json
{
  "[python]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
      "source.organizeImports": "explicit"
    },
    "editor.defaultFormatter": "charliermarsh.ruff"
  }
}
```

#### Example Before and After

Before:

```python
from datetime import datetime
import os
import sys
from typing import List,Dict
def my_function(  x: int,y: int  )->int:
    return x+y
```

After Ruff formatting:

```python
import os
import sys
from datetime import datetime
from typing import Dict, List


def my_function(x: int, y: int) -> int:
    return x + y
```
