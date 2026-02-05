---
applyTo: 'justfile'
description: 'Just recipe runner conventions and best practices for project task automation'
---

# Just (justfile) Instructions

## What is Just?

`just` is a command runner for saving and running project-specific recipes. It's similar to `make` but is a command runner, not a build system, making it simpler and more straightforward.

Key features:
- **No `.PHONY` directives needed** — Recipes are commands, not build targets
- **Cross-platform support** — Works on Linux, macOS, Windows without additional dependencies
- **Recipe parameters** — Recipes accept command-line arguments with defaults and variadic support
- **Arbitrary languages** — Recipes can use Python, Node, shell scripts, etc. via shebangs
- **Optional `.env` loading** — Environment variables loaded from `.env` when `set dotenv-load` is enabled
- **Shell completion** — Available for most popular shells
- **Static error resolution** — Unknown recipes and circular dependencies are caught before execution

## Recipe Syntax & Structure

### Basic Recipe Definition

```justfile
# A simple recipe with no parameters
task-name:
    command1
    command2

# Recipe with parameters
build target:
    gcc {{target}}.c -o {{target}}

# Recipe with default parameter value
test filter="":
    pytest {{filter}}
```

### Recipe Features

1. **Indentation**: Use spaces OR tabs consistently within each recipe (different recipes may differ)
2. **Variables**: Use `{{variable_name}}` to interpolate variables in recipe bodies
3. **Parameters**: Define with `recipe-name param1 param2:` syntax
4. **Documentation comments**: Comments immediately preceding a recipe appear in `just --list`
5. **Shebang recipes**: Multi-line scripts using `#!/usr/bin/env python3`, `#!/usr/bin/env node`, etc.

### Quiet Recipes and Lines

```justfile
# Prefix recipe name with @ to suppress command echoing for all lines
@quiet-recipe:
    echo "commands not echoed, only output shown"

# Prefix individual lines with @ to suppress that line's echo
verbose-recipe:
    echo "this line IS echoed before running"
    @echo "this line is NOT echoed, only output"

# Use set quiet for all recipes
set quiet
```

### Private Recipes

```justfile
# Prefix with _ to hide from just --list
_helper:
    internal-command

# Or use [private] attribute
[private]
internal-helper:
    command
```

### Recipe Attributes

```justfile
# Require confirmation before running
[confirm]
dangerous-operation:
    rm -rf {{target}}

# Suppress exit error messages (useful for wrapper recipes)
[no-exit-message]
git *args:
    @git {{args}}

# Set working directory for a recipe
[working-directory: './subdir']
subdir-task:
    command

# Override quiet setting
[no-quiet]
loud-recipe:
    echo "always echoed"
```

## Common Justfile Patterns

### 1. Default Recipe and Listing

```justfile
# Default recipe runs when no recipe specified
default:
    @just --list

# Or make another recipe the default
default: build
```

### 2. Variable Definition

```justfile
# Simple variables
VERSION := "1.0.0"
RELEASE := "./dist"

# Command output as variable (backtick evaluation)
TIMESTAMP := `date +%s`
GIT_HASH := `git rev-parse --short HEAD`

# Path joining with /
CONFIG := config_dir() / ".project-config"

# Environment variable with default
HOME_DIR := env("HOME", "/tmp")
```

### 3. Dependencies

```justfile
# Recipe depends on others (run in order)
deploy: build test
    ./scripts/deploy.sh

# Pass arguments to dependencies
default: (build "main")

build target:
    @echo 'Building {{target}}…'

# Chain dependencies
push target: (build target)
    @echo 'Pushing {{target}}…'
```

### 4. Shebang Recipes

```justfile
# Multi-line bash script
build:
    #!/usr/bin/env bash
    set -euo pipefail
    VERSION=$(just version)
    goreleaser build --single-target --snapshot --clean

# Python script
analyze:
    #!/usr/bin/env python3
    import json
    with open('data.json') as f:
        data = json.load(f)
    print(f"Found {len(data)} items")

# Node script
validate:
    #!/usr/bin/env node
    const fs = require('fs');
    const pkg = JSON.parse(fs.readFileSync('package.json'));
    console.log(`Validating ${pkg.name}...`);
```

### 5. Parameter Variations

```justfile
# Required parameter
build target:
    gcc {{target}}.c -o {{target}}

# Optional with default
test filter="*":
    pytest tests/{{filter}}_test.py

# Variadic: one or more arguments (+)
backup +FILES:
    scp {{FILES}} server.com:

# Variadic: zero or more arguments (*)
commit MESSAGE *FLAGS:
    git commit {{FLAGS}} -m "{{MESSAGE}}"

# Export parameter as environment variable
foo $BAR:
    echo $BAR
```

### 6. Settings

```justfile
# Shell configuration (default is sh -cu)
set shell := ["bash", "-cu"]

# Load .env file (NOT automatic by default)
set dotenv-load

# Export all variables as environment variables
set export

# Pass recipe arguments as positional shell arguments
set positional-arguments

# Quiet mode for all recipes
set quiet
```

## Best Practices

### 1. Recipe Naming
- Use **kebab-case**: `build-docker`, `run-tests`, `deploy-prod`
- Prefix helper recipes with `_` to hide from `just --list`
- Use descriptive, action-oriented names

### 2. Documentation Comments
```justfile
# Build the application for production
build:
    cargo build --release

# Run all tests with optional filter
test filter="":
    cargo test {{filter}}
```

Comments immediately before recipes appear in `just --list`:
```
Available recipes:
    build # Build the application for production
    test  # Run all tests with optional filter
```

### 3. Consistent Structure
```justfile
# Settings at the top
set shell := ["bash", "-cu"]
set dotenv-load

# Variables next
VERSION := "1.0.0"

# Default recipe
default: build

# === Build Recipes ===

build:
    cargo build

# === Test Recipes ===

test:
    cargo test

# === Private Helpers ===

_setup:
    mkdir -p build
```

### 4. Shebang Recipes for Complex Logic
For multi-line scripts or complex logic, use shebang recipes instead of line continuation:

```justfile
# Good: shebang for complex logic
deploy env:
    #!/usr/bin/env bash
    set -euo pipefail
    if [[ "{{env}}" == "prod" ]]; then
        echo "Deploying to production"
        ./deploy-prod.sh
    else
        echo "Deploying to staging"
        ./deploy-stage.sh
    fi

# Avoid: complex line continuation
deploy-awkward env:
    @if [ "{{env}}" = "prod" ]; then \
        echo "Deploying to production"; \
    fi
```

### 5. Use @ for Clean Output
```justfile
# Suppress command echo for cleaner output
@build:
    echo "Building..."
    cargo build

# Or suppress specific lines
test:
    @echo "Running tests..."
    cargo test
```

## Interaction Patterns for AI Agents

When working with justfiles:

1. **Match existing indentation** — Check if the file uses spaces or tabs, maintain consistency
2. **Preserve recipe ordering** — Maintain logical grouping (build, test, deploy, etc.)
3. **Check for dependencies** — Ensure new recipes don't break existing dependency chains
4. **Use documentation comments** — Add comments before recipes for `just --list` output
5. **Match shell conventions** — Check `set shell` and match existing recipe patterns
6. **Prefer shebang recipes** — For complex multi-line logic, use shebang instead of line continuation

## Anti-Patterns to Avoid

1. **Mixed indentation within a recipe** — Pick spaces or tabs, be consistent per recipe
2. **Make-specific features** — Don't use `.PHONY`, automatic variables, or implicit rules
3. **Assuming dotenv loads automatically** — Must explicitly `set dotenv-load`
4. **Complex line continuation** — Use shebang recipes for multi-line scripts
5. **Hardcoded paths** — Use variables, `env()`, or path functions
6. **Silent failures** — Provide feedback on operations; use `[no-exit-message]` intentionally
7. **Overly broad recipes** — Keep recipes focused and composable
