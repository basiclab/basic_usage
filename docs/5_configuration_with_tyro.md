# ⚙️ Configuration with Tyro

>[!TIP]
>If you're already using a configuration tool like `click`, `yacs`, or `hydra`, feel free to skip this section!

## 🎯 Why Configuration Tools?

* **Single Source of Truth**: Avoid duplicating option definitions in code, parsers, and docs.
* **Less Boilerplate**: Auto‐generate parsing, defaults, validation, and help text.

## 💫 Why Tyro?

* **Pure Python**: Uses type hints/dataclasses—no extra DSL.
* **One-liner CLI**: `tyro.cli(...)` infers flags, defaults, help, and subcommands.
* **Better DX**: IDE autocomplete, type checking, and consistent, hierarchical interfaces. (my favorite 🤗)

## 🚀 How to Use Tyro

At its core, Tyro's entry point is `tyro.cli()`, which takes either:

1. **function** (Used for simple script)

   Tyro generates flags corresponding to the function's parameters, parses them, and then calls your function directly.

   ```python
   import tyro

   def main(input: str, verbose: bool = False) -> None:
       print(f"Input: {input}, verbose={verbose}")

   if __name__ == "__main__":
       tyro.cli(main)
   ```

   Running `python script.py --help` will automatically show:

   ```bash
   usage: test.py [-h] --input STR [--verbose | --no-verbose]

    ╭─ options ──────────────────────────────────────────╮
    │ -h, --help         show this help message and exit │
    │ --input STR        (required)                      │
    │ --verbose, --no-verbose                            │
    │                    (default: False)                │
    ╰────────────────────────────────────────────────────╯
   ```

2. **dataclass (or other type)** (used for detailed settings)

   Tyro populates an instance of your dataclass from the CLI and returns it for further use.

   ```python
   from dataclasses import dataclass
   import tyro

   @dataclass
   class Config:
       input_path: str
       batch_size: int = 32

   if __name__ == "__main__":
       cfg = tyro.cli(Config)
       print(f"Running with config: {cfg}")
   ```

   Here, `tyro.cli(Config)` reads `--input-path` and `--batch-size` flags and returns a `Config` instance.

## ⚡ Using Default Configs & Overriding Them

### 🔄 Overriding Defaults in Dataclasses

You can pre-instantiate a dataclass with certain defaults and then let Tyro require or override only the missing parts by using the `default=` argument:

```python
from dataclasses import dataclass
import tyro

@dataclass
class Args:
    string: str
    reps: int = 3

if __name__ == "__main__":
    args = tyro.cli(
        Args,
        default=Args(string="hello", reps=tyro.MISSING),
    )
    print(args)
```

* Here, `--string` will default to `"hello"`.
* `reps` is marked `MISSING`, so it becomes **required** on the CLI (`--reps INT`) until you explicitly provide it.

## 📊 Hierarchical Dataclasses

Tyro can also be used for hierarchical dataclasses for easy usage:

```python
import dataclasses
import pathlib

import tyro


@dataclasses.dataclass
class OptimizerConfig:
    learning_rate: float = 3e-4
    weight_decay: float = 1e-2


@dataclasses.dataclass
class Config:
    # Optimizer options.
    optimizer: OptimizerConfig

    # Save directory.
    save_dir: str = "logs"

    # Random seed.
    seed: int = 0


def train(config: Config) -> None:
    ### .... Training a model ...
    pass


if __name__ == "__main__":
    config = tyro.cli(Config)
    train(config)
    print(config)
```

### 📝 Help message

```shell
$ python script.py --help
usage: script.py [-h] [OPTIONS]

╭─ options ─────────────────────────────────────────────╮
│ -h, --help            show this help message and exit │
│ --save-dir STR        Save directory. (default: logs) │
│ --seed INT            Random seed. (default: 0)       │
╰───────────────────────────────────────────────────────╯
╭─ optimizer options ───────────────────────────────────╮
│ Optimizer options.                                    │
│ ───────────────────────────────────────               │
│ --optimizer.learning-rate FLOAT                       │
│                       (default: 0.0003)               │
│ --optimizer.weight-decay FLOAT                        │
│                       (default: 0.01)                 │
╰───────────────────────────────────────────────────────╯
```

### 💻 Usage

Users can choose using the default value or overwrite with the arguments:

```shell
# using the default
$ python script.py
Config(optimizer=OptimizerConfig(learning_rate=0.0003, weight_decay=0.01), save_dir='logs', seed=0)

# overwrite
$ python script.py --save_dir runs --seed 1234 --optimizer.learning-rate 1e-5
Config(optimizer=OptimizerConfig(learning_rate=1e-05, weight_decay=0.01), save_dir='runs', seed=1234)
```

## 💾 Saving the configs

Saving the config files allow other users to reuse them. To save the config file, users are allow to save it in the `JSON` or `YAML` format. Let's use the same example:

```python
import dataclasses
import os

import tyro
import yaml


@dataclasses.dataclass
class OptimizerConfig:
    learning_rate: float = 3e-4
    weight_decay: float = 1e-2


@dataclasses.dataclass
class Config:
    # Optimizer options.
    optimizer: OptimizerConfig

    # Save directory.
    save_dir: str = "logs"

    # Random seed.
    seed: int = 0


def train(config: Config) -> None:
    ### .... Training a model ...
    pass


if __name__ == "__main__":
    config = tyro.cli(Config)
    train(config)
    os.makedirs(config.save_dir, exist_ok=True)
    with open(os.path.join(config.save_dir, "config.yaml"), "w") as f:
        yaml.safe_dump(
            dataclasses.asdict(config),
            f,
        )
```

## 🔄 Other alternatives

It is recommended to take [Hydra](https://github.com/facebookresearch/hydra) a look which is designed by Meta. It is good for designing a **complex experiments in an elegant way**.

## 🎓 Advanced Tips

All the advanced tips are included in the [documents](https://brentyi.github.io/tyro/)! Please check it out!.
