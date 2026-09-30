<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🐍 Patch Python Project Version

Updates the Python project version in the relevant repository file.

Supports the following metadata description files/locations:

- pyproject.toml
- setup.py

## Features

- **Automatic Version Normalization**: Strips leading `v` or `V` prefix from
  version strings (e.g., `v1.0.0` → `1.0.0`)
- **PEP 440 Validation**: Rejects version strings that do not follow Python
  packaging standards
- **Multi-file Support**: Patches both modern (`pyproject.toml`) and legacy
  (`setup.py`) project files

## python-project-version-patch-action

## Usage Examples

### Basic Usage

```yaml
# Code checkout performed in earlier workflow step
- name: Patch the current Python Project version
  uses: lfreleng-actions/python-project-version-patch-action@main
  with:
    replacement_version: '0.2.0'
```

### With Git Tag (v prefix)

The action automatically strips the leading `v` from Git tags:

```yaml
- name: Patch version from Git tag
  uses: lfreleng-actions/python-project-version-patch-action@main
  with:
    # Input: v1.2.3 → Output in files: 1.2.3
    replacement_version: 'v1.2.3'
```

### With Path Prefix

```yaml
- name: Patch version in subdirectory
  uses: lfreleng-actions/python-project-version-patch-action@main
  with:
    replacement_version: '2.0.0'
    path_prefix: 'src/myproject'
```

## Inputs

<!-- markdownlint-disable MD013 -->

| Input Variable      | Required | Default | Description                                         |
| ------------------- | -------- | ------- | --------------------------------------------------- |
| replacement_version | Yes      | -       | Version string to set (e.g., `1.0.0` or `v1.0.0`)   |
| path_prefix         | No       | `.`     | Directory path to the repository/project files      |

<!-- markdownlint-enable MD013 -->

## Version Normalization

The action automatically normalizes version strings to match Python packaging
standards:

- **Strips `v` prefix**: `v1.0.0` → `1.0.0`
- **Strips `V` prefix**: `V2.0.0` → `2.0.0`
- **Validates PEP 440**: Fails the action if the version format is invalid

This allows you to pass Git tags (which commonly use `v` prefix) directly to
the action without manual preprocessing.

## Notes

- The action patches version strings in both `setup.py` and `pyproject.toml`
  if both files exist
- Version validation follows the PEP 440 grammar (e.g., `1.0.0`, `1.0.0a1`,
  `1.0.0.dev0`, `1!2.0.0`, `1.0.0+local`)
- Invalid version formats fail the action, and leave the project files
  unchanged
- In `pyproject.toml`, the action replaces the first line starting
  `version =` with `version = "<version>"`
- In `setup.py`, the action replaces the quoted string on the first
  `version=` line whose value is a single string literal, keeping its
  indentation, any trailing comma, closing parenthesis or comment; a
  non-literal value such as `version=get_version()` or
  `version="1.0" if x else "2.0"` fails the action
- The action fails if the file is empty or contains no matching `version`
  line
