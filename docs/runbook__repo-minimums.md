# Runbook: Minimum Repo Files and CI Setup

**Role:** DevOps

This runbook captures the minimum set of files each Issues-FS repo should have, and where to copy them from. It is designed so a fresh LLM session can bootstrap a new repo with the standard layout.

## Source Template

Use this repo as the template:

```
modules/Issues-FS
```

## Required Files and Folders

Copy these from `modules/Issues-FS` into the target repo:

- `.github/workflows/ci-pipeline.yml`
- `.github/workflows/ci-pipeline__dev.yml`
- `.github/workflows/ci-pipeline__main.yml`
- `requirements-test.txt`
- `scripts/gh-release-to-main.sh`

## Required Python Package Skeleton

Create a minimal package layout to support versioning and CI tests.

Example for repo `Issues-FS__CLI` with package `issues_fs_cli`:

```
issues_fs_cli/
├── __init__.py
├── version
└── utils/
    ├── __init__.py
    └── Version.py
```

### `__init__.py`

```python
package_name = 'issues_fs_cli'
path         = __path__[0]
```

### `version`

```
v0.1.0
```

### `utils/Version.py`

```python
import issues_fs_cli
from osbot_utils.type_safe.primitives.domains.common.safe_str.Safe_Str__Version import Safe_Str__Version
from osbot_utils.type_safe.Type_Safe                                            import Type_Safe
from osbot_utils.utils.Files                                                    import file_contents, path_combine


class Version(Type_Safe):

    FILE_NAME_VERSION = 'version'

    def path_code_root(self):
        return issues_fs_cli.path

    def path_version_file(self):
        return path_combine(self.path_code_root(), self.FILE_NAME_VERSION)

    def value(self):
        version = file_contents(self.path_version_file()) or ""
        return Safe_Str__Version(version)


version__issues_fs_cli = Version().value()
```

## Required Tests

Create the minimal test that matches the Version class.

```
tests/unit/utils/test_Version.py
```

```python
import issues_fs_cli
from unittest                         import TestCase
from osbot_utils.utils.Files          import parent_folder, file_name
from issues_fs_cli.utils.Version      import version__issues_fs_cli, Version


class test_Version(TestCase):

    @classmethod
    def setUpClass(cls):
        cls.version = Version()

    def test_path_code_root(self):
        assert self.version.path_code_root() == issues_fs_cli.path

    def test_path_version_file(self):
        with self.version as _:
            assert parent_folder(_.path_version_file()) == issues_fs_cli.path
            assert file_name    (_.path_version_file()) == 'version'

    def test_value(self):
        assert self.version.value() == version__issues_fs_cli
```

## Required `pyproject.toml`

Copy `modules/Issues-FS/pyproject.toml` and update:

- `name` to the new package name (snake_case)
- `description` to the repo name
- `homepage` and `repository` to the new GitHub URL

Example for `Issues-FS__CLI`:

```toml
[tool.poetry]
name        = "issues_fs_cli"
version     = "v0.1.0"
description = "Issues-FS__CLI"
authors     = ["Dinis Cruz <dinis.cruz@owasp.org>"]
license     = "Apache 2.0"
readme      = "README.md"
homepage    = "https://github.com/owasp-sbot/Issues-FS__CLI"
repository  = "https://github.com/owasp-sbot/Issues-FS__CLI"

[tool.poetry.dependencies]
python            = "^3.12"
osbot-utils       = "*"
memory_fs         = "*"

[build-system]
requires = ["poetry-core>=1.9.1"]
build-backend = "poetry.core.masonry.api"
```

## CI Workflow Adjustment

In `.github/workflows/ci-pipeline.yml`, update:

```
PACKAGE_NAME: 'issues_fs'
```

to the new package name, e.g.:

```
PACKAGE_NAME: 'issues_fs_cli'
```

## README

Use a short README with:

- Project title
- PyPI badge
- Install command
- Dev setup
- Test command

## Final Checklist

- `.github/workflows/*` present
- `pyproject.toml` updated
- `requirements-test.txt` copied
- `scripts/gh-release-to-main.sh` copied
- package skeleton created
- `version` file created
- Version tests added
- `PACKAGE_NAME` updated in CI
- README created
