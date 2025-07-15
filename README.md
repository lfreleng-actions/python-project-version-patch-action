<!--
SPDX-License-Identifier: Apache-2.0
SPDX-FileCopyrightText: 2025 The Linux Foundation
-->

# 🐍 Patch Python Project Version

Updates the Python project version in the relevant repository file.

Supports the following metadata description files/locations:

- pyproject.toml
- setup.py

## python-project-version-patch-action

## Usage Example

An example workflow step using this action:

```yaml
  # Code checkout performed in earlier workflow step
  - name: Patch the current Python Project version
    uses: lfreleng-actions/python-project-version-patch-action@main
    with:
      replacement_version: '0.2.0'
```

## Inputs

| Output Variable     | Description                                    |
| ------------------- | ---------------------------------------------- |
| replacement_version | Replacement version of the Python Project      |
| path_prefix         | Directory path to the repository/project files |
