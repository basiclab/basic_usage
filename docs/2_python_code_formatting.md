# ✨ Python Code Formatting with Black and isort

This guide explains how to use Black and isort to automatically format your Python code, ensuring consistency and readability across your project.

## 🎯 What are Black and isort?

- **Black**: An opinionated Python code formatter that automatically formats your code to comply with PEP 8 style guide
- **isort**: A Python utility that sorts your imports alphabetically and separates them into sections

> ⚠️ **Important**: Black and isort can conflict with each other's formatting. Always configure isort to be compatible with Black's formatting style.

## 📦 Installation

```bash
# Install both tools using pip
pip install black isort
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

Create a `pyproject.toml` file in your project root:

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
    "python.formatting.provider": "black",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
        "source.organizeImports": true
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

## 🔧 Pre-commit Hook Setup (Optional)

1. Install pre-commit:

```bash
pip install pre-commit
```

2. Create `.pre-commit-config.yaml`:

```yaml
repos:
-   repo: https://github.com/psf/black
    rev: 23.12.1
    hooks:
    -   id: black
-   repo: https://github.com/pycqa/isort
    rev: 5.13.2
    hooks:
    -   id: isort
```

3. Install the hooks:

```bash
pre-commit install
```

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

## 🔍 Advanced Usage

Users can also choose [ruff](https://github.com/astral-sh/ruff) as an alternative to Black and isort. We will cover it in another guide.
