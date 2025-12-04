![tests](https://github.com/ldkv/taskfiles/actions/workflows/test-taskfiles.yml/badge.svg?branch=main)

# taskfiles

## Overview

A collection of useful [Taskfiles](https://taskfile.dev/) that can be included in your own projects using the [Remote Taskfiles](https://taskfile.dev/docs/experiments/remote-taskfiles) feature.

All commands are included in [all.yml](./collections/all.yml) for easy inclusion in your own projects

It is also possible to include individual collections as needed.

### [python-collection](./collections/python/python.yml)

A collection of Taskfiles for Python projects.

- [`setup.yml`](./collections/python/setup.yml): Tasks for setting up Python development environments.
- [`checks.yml`](./collections/python/checks.yml): Tasks for checking code quality and dependencies.
- [`django.yml`](./collections/python/django.yml): Tasks for managing Django projects via `manage.py`.

### [docker-collection](./collections/docker.yml)

A collection of commands for Docker and docker-compose.

## Usage

To include these remote Taskfiles, add the following section into your own `Taskfile.yml`:

```yaml
includes:
  all-collections: # For all available tasks
    taskfile: "https://github.com/ldkv/taskfiles.git//collections/Taskfile.yml?ref=refs/tags/v1"
    dir: .
    flatten: true
  python-collection: # For only Python-related tasks
    taskfile: "https://github.com/ldkv/taskfiles.git//collections/python/Taskfile.yml?ref=refs/tags/v1"
    dir: .
```

## Configure Remote Taskfiles

### Enable the feature

Take a look at [.taskrc.yml](./.taskrc.yml) for configuration options related to remote Taskfiles.

```yaml
# Global settings
verbose: true
concurrency: 4

# Enable experimental features
experiments:
  REMOTE_TASKFILES: 1 # The feature is disabled by default

remote:
  insecure: false
  offline: false
  timeout: "30s"
  cache-expiry: "24h" # Duration before re-fetching remote Taskfiles
```

This configuration can be globally applied by placing it in your home directory (`~/.taskrc.yml`), or locally in your project's root directory, together with your `Taskfile.yml`.

An alternative way to enable the feature globally is to set the environment variable `TASK_X_REMOTE_TASKFILES=1`.

### Customize Cache Directory

By default, the remote taskfiles are cached in the working directory (same origin as your own `Taskfile`).

It is possible to customize the cache directory via [environment variables](https://taskfile.dev/docs/experiments/remote-taskfiles) `TASK_REMOTE_DIR`. For example:

```shell
export TASK_REMOTE_DIR="$HOME/.cache/taskfiles"
```

Add this line to your shell profile (e.g., `~/.bashrc` or `~/.zshrc`) to make it permanent.

### Force Re-fetching Remote Taskfiles

To force re-fetching remote Taskfiles, regardless of the cache expiry setting, run:

```shell
task --download
```
