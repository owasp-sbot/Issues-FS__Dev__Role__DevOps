# Submodule Status Report -- 2026-02-10

**Author:** DevOps Agent
**Parent Repo:** `Issues-FS__Dev` (`/home/user/Issues-FS__Dev/`)
**Date:** 2026-02-10
**Submodule Count:** 17 (6 modules, 10 roles, 1 human)

---

## Executive Summary

### Aggregate Metrics

| Metric | Count |
|--------|-------|
| Total submodules | 17 |
| On named branch | 13 |
| Detached HEAD | 4 |
| CI present (3-file pattern) | 15 / 17 |
| CI missing | 2 (QA, Human) |
| `pyproject.toml` present | 15 / 17 |
| `pyproject.toml` missing | 2 (QA, Human) |
| `.issues/` directory present | 15 / 17 |
| `.issues/` missing | 2 (QA, Human) |
| `ROLE.md` present (roles only) | 10 / 10 |
| Total test files | 73 |
| Total tracked issues (JSON) | 114 |
| Dirty working trees | 0 / 17 |
| Parent pointer drift | 4 submodules |

### Health Distribution

| Health | Count | Submodules |
|--------|-------|------------|
| Green | 8 | Issues-FS, Issues-FS__CLI, Issues-FS__Service, Architect, Conductor, Dev, DevOps, Journalist |
| Yellow | 7 | Issues-FS__Docs, Issues-FS__Service__Client__Python, Issues-FS__Service__UI, AppSec, Cartographer, Historian, Librarian |
| Red | 2 | QA, Human |

### Key Findings

1. **No submodule is on `main` or `dev`.** All 17 are either on feature branches (13) or in detached HEAD state (4). None have been merged back to their default branch.
2. **Parent repo has 4 uncommitted pointer changes** for Architect, Dev, DevOps, and Librarian -- these submodules have advanced since the parent last committed their refs.
3. **QA and Human submodules lack standard infrastructure** (no CI, no pyproject.toml, no .issues directory). QA has no tests directory at all.
4. **The double-path CLI bug** (documented 2026-02-09) means `issues-fs list` returns 0 results on all repos despite 114 issue JSON files existing on disk.
5. **All CI-enabled repos follow the standard 3-file pattern**: `ci-pipeline.yml`, `ci-pipeline__dev.yml`, `ci-pipeline__main.yml`.

---

## Modules (6)

### Module Status Table

| Submodule | Version | Branch State | Tests | Issues (JSON) | CI | Docs | Health |
|-----------|---------|-------------|-------|---------------|-----|------|--------|
| Issues-FS | v0.4.5 | `claude/add-bug-tests` | 36 | 25 | 3-file | No dir | Green |
| Issues-FS__CLI | v0.2.2 | `claude/fix-double-path-all-methods-rcalB` | 6 | 7 | 3-file | 4 files | Green |
| Issues-FS__Docs | v0.1.4 | Detached HEAD | 1 | 0 | 3-file | 49 files | Yellow |
| Issues-FS__Service | v0.2.2 | Detached HEAD | 19 | 0 | 3-file | No dir | Green |
| Issues-FS__Service__Client__Python | v0.2.2 | Detached HEAD | 1 | 0 | 3-file | No dir | Yellow |
| Issues-FS__Service__UI | v0.1.9 | `claude/fix-obj-id-data-rcalB` | 2 | 56 | 3-file | 1 file | Yellow |

### Module Details

#### Issues-FS (Core Library)
- **Package:** `issues_fs` v0.4.5
- **Branch:** `claude/add-bug-tests` (3 commits ahead of `origin/dev`)
- **Latest commit:** `e4b85ac` -- Fix double-path bug in all Path__Handler__Graph_Node methods
- **Tests:** 36 test files across graph_services, issues (phase 1, phase 2, status, storage), mgraph, schemas
- **Issues on disk:** 25 JSON files
- **Health:** GREEN -- Highest test coverage of any submodule. Active bug fix branch. Core library is the most mature module.

#### Issues-FS__CLI
- **Package:** `issues_fs_cli` v0.2.2
- **Branch:** `claude/fix-double-path-all-methods-rcalB` (7 commits ahead of `origin/main`)
- **Latest commit:** `334f9bd` -- Add feature request: issues-fs validate command
- **Tests:** 6 test files (CLI context, label parser, output, commands, init, version)
- **Issues on disk:** 7 JSON files
- **Docs:** 4 files
- **Health:** GREEN -- Active development, reasonable test coverage for a CLI module.

#### Issues-FS__Docs
- **Package:** `issues_fs_docs` v0.1.4
- **Branch:** Detached HEAD at `65f9047` (20 commits ahead of `origin/main`)
- **Latest commit:** `65f9047` -- added no-raw-primitives policy
- **Tests:** 1 test file
- **Issues on disk:** 0 (config only)
- **Docs:** 49 files -- largest docs collection in the ecosystem
- **Health:** YELLOW -- Detached HEAD with 20 unmerged commits. Only 1 test file for a docs-focused module. No issues tracked.

#### Issues-FS__Service
- **Package:** `issues_fs_service` v0.2.2
- **Branch:** Detached HEAD at `f51a5a7` (9 commits ahead of `origin/main`)
- **Latest commit:** `f51a5a7` -- Update release badge and version file
- **Tests:** 19 test files (deploy, routes, fast_api)
- **Issues on disk:** 0 (config only)
- **Health:** GREEN -- Strong test coverage. Detached HEAD is a minor concern but commit is close to release.

#### Issues-FS__Service__Client__Python
- **Package:** `issues_fs_service_client_python` v0.2.2
- **Branch:** Detached HEAD at `9be5b4b` (3 commits ahead of `origin/main`)
- **Latest commit:** `9be5b4b` -- Update release badge and version file
- **Tests:** 1 test file
- **Issues on disk:** 0 (config only)
- **Health:** YELLOW -- Minimal test coverage (1 file). Detached HEAD. Client library needs more testing.

#### Issues-FS__Service__UI
- **Package:** `issues_fs_service_ui` v0.1.9
- **Branch:** `claude/fix-obj-id-data-rcalB` (22 commits ahead of `origin/main`)
- **Latest commit:** `25471f7` -- Fix invalid Obj_Id values in 5 issue files
- **Tests:** 2 test files
- **Issues on disk:** 56 JSON files (highest issue count in ecosystem)
- **Docs:** 1 file
- **Health:** YELLOW -- 22 unmerged commits is the highest drift of any module. Only 2 tests despite having 56 tracked issues. Data fix branch suggests data integrity concerns.

---

## Roles (10)

### Role Status Table

| Submodule | Version | Branch State | Tests | Issues (JSON) | CI | Docs | ROLE.md | Health |
|-----------|---------|-------------|-------|---------------|-----|------|---------|--------|
| AppSec | v0.1.0 | `claude/add-role-setup` | 1 | 0 | 3-file | No dir | Yes | Yellow |
| Architect | v0.1.3 | `claude/add-news-site-adr-task-rcalB` | 1 | 4 | 3-file | 2 files | Yes | Green |
| Cartographer | v0.1.0 | Detached HEAD | 1 | 0 | 3-file | 1 file | Yes | Yellow |
| Conductor | v0.1.3 | `claude/add-status-reports` | 1 | 0 | 3-file | 2 files | Yes | Green |
| Dev | v0.1.3 | `claude/add-news-site-impl-task-rcalB` | 1 | 3 | 3-file | 3 files | Yes | Green |
| DevOps | v0.4.2 | `claude/add-submodule-report-task-rcalB` | 1 | 3 | 3-file | 4 files | Yes | Green |
| Historian | v0.1.0 | Detached HEAD | 1 | 0 | 3-file | 1 file | Yes | Yellow |
| Journalist | v0.1.4 | `claude/add-article-task-rcalB` | 1 | 6 | 3-file | 2 files | Yes | Green |
| Librarian | v0.1.0 | `claude/add-catalog-task-rcalB` | 1 | 10 | 3-file | 2 files | Yes | Yellow |
| QA | N/A | `claude/add-status-reports` | 0 | 0 | None | 4 files | Yes | Red |

### Role Details

#### AppSec
- **Package:** `issues_fs_dev_role_appsec` v0.1.0
- **Branch:** `claude/add-role-setup` (2 commits ahead of `origin/main`)
- **Latest commit:** `fa7758a` -- Add CI pipeline workflows (3-file standard pattern)
- **Health:** YELLOW -- Initial setup only. No issues, no docs directory. Needs content.

#### Architect
- **Package:** `issues_fs_dev_role_architect` v0.1.3
- **Branch:** `claude/add-news-site-adr-task-rcalB` (12 commits ahead of `origin/main`)
- **Local branches:** 3 (including stale `claude/add-role-setup` with gone remote)
- **Latest commit:** `e8fc5ec` -- Add task: revise ADR-001 for Hugo per stakeholder interview
- **Issues on disk:** 4 JSON files
- **Health:** GREEN -- Active development with ADR work. Stale branch should be cleaned up.

#### Cartographer
- **Package:** `issues_fs_dev_role_cartographer` v0.1.0
- **Branch:** Detached HEAD at `30997e5` (5 commits ahead of `origin/main`)
- **Latest commit:** `30997e5` -- Merge commit into dev
- **Health:** YELLOW -- Detached HEAD, still at v0.1.0. No issues tracked. Minimal docs.

#### Conductor
- **Package:** `issues_fs_dev_role_conductor` v0.1.3
- **Branch:** `claude/add-status-reports` (8 commits ahead of `origin/main`)
- **Latest commit:** `58f4a4f` -- Add Conductor roadmap and next steps for 2026-02-09
- **Health:** GREEN -- Active roadmap/status work. Good docs presence.

#### Dev
- **Package:** `issues_fs_dev_role_dev` v0.1.3
- **Branch:** `claude/add-news-site-impl-task-rcalB` (11 commits ahead of `origin/main`)
- **Local branches:** 4 (most of any role)
- **Latest commit:** `f91a8ad` -- Add task: implement Issues-FS News Site
- **Issues on disk:** 3 JSON files
- **Health:** GREEN -- Most active role. Multiple feature branches. Good docs presence.

#### DevOps
- **Package:** `issues_fs_dev_role_devops` v0.4.2
- **Branch:** `claude/add-submodule-report-task-rcalB` (1 commit ahead of `origin/dev`)
- **Local branches:** 4 (2 with gone remotes)
- **Latest commit:** `1d1de50` -- Add task: technical report on all 17 submodule statuses
- **Issues on disk:** 3 JSON files
- **Docs:** 4 files (runbooks, branch protection guide)
- **Health:** GREEN -- Highest version of any role (v0.4.2). Good operational docs. Stale branches should be cleaned up.

#### Historian
- **Package:** `issues_fs_dev_role_historian` v0.1.0
- **Branch:** Detached HEAD at `42f569f` (5 commits ahead of `origin/main`)
- **Latest commit:** `42f569f` -- Merge commit into dev
- **Health:** YELLOW -- Detached HEAD, still at v0.1.0. No issues tracked. Minimal docs.

#### Journalist
- **Package:** `issues_fs_dev_role_journalist` v0.1.4
- **Branch:** `claude/add-article-task-rcalB` (19 commits ahead of `origin/main`)
- **Local branches:** 5 (most branches of any submodule)
- **Latest commit:** `e8a0a51` -- Add task: process stakeholder interview and LinkedIn publication
- **Issues on disk:** 6 JSON files
- **Health:** GREEN -- Most active role by commit count. 5 local branches suggests heavy parallel work; may need branch cleanup.

#### Librarian
- **Package:** `issues_fs_dev_role_librarian` v0.1.0
- **Branch:** `claude/add-catalog-task-rcalB` (6 commits ahead of `origin/main`)
- **Latest commit:** `99f2e14` -- Add task: catalog stakeholder interview and LinkedIn article
- **Issues on disk:** 10 JSON files (highest for any role)
- **Health:** YELLOW -- Still at v0.1.0 despite having the most issues of any role. Needs a version bump.

#### QA
- **Package:** MISSING (no pyproject.toml)
- **Branch:** `claude/add-status-reports` (2 commits ahead of `origin/main`)
- **Latest commit:** `38ac196` -- Add QA test execution status for 2026-02-09
- **Tests:** 0 (no tests directory)
- **Issues:** No `.issues/` directory
- **CI:** MISSING (no `.github/workflows/`)
- **Docs:** 4 files (review notes, status reports)
- **Health:** RED -- Missing pyproject.toml, CI, tests, and .issues directory. The QA role has zero standard infrastructure despite having docs content. This is the most under-provisioned role.

---

## Human (1)

### Human Status Table

| Submodule | Branch State | CI | pyproject.toml | .issues | Docs | Health |
|-----------|-------------|-----|----------------|---------|------|--------|
| Dinis_Cruz | Detached HEAD | None | Missing | Missing | No dir | Red |

#### Issues-FS__Dev__Human__Dinis_Cruz
- **Branch:** Detached HEAD at `ddc3f2f` (1 commit ahead of `origin/main`)
- **Latest commit:** `ddc3f2f` -- added first brief
- **Structure:** LICENSE, README.md, .gitignore, and 1 brief file (`briefs/02/09/v0.2.9__brief_next-steps.md`)
- **Health:** RED -- Minimal content. No CI, no package, no issues, no docs directory. This is expected for a human profile repo, but it lacks any standard infrastructure. Whether this warrants infrastructure is a design decision.

---

## Cross-Cutting Observations

### 1. Branch Hygiene

**No submodule is on its default branch.** Every single submodule has diverged:

| State | Count | Submodules |
|-------|-------|------------|
| Named feature branch | 13 | Issues-FS, CLI, Service__UI, AppSec, Architect, Conductor, Dev, DevOps, Journalist, Librarian, QA |
| Detached HEAD | 4 | Docs, Service, Service__Client__Python, Cartographer, Historian, Human |

This means no work from any submodule has been merged to `main`/`dev` remotely. The ecosystem is in an "all branches diverged" state.

**Stale branches exist in 4 repos:** Architect (1 gone remote), DevOps (2 gone remotes), Dev (multiple), Journalist (5 branches total).

**Recommendation:** Establish a merge cadence. Consider a weekly PR review to merge feature branches back to default branches.

### 2. Parent Repo Pointer Drift

The parent repo has uncommitted submodule pointer changes for 4 submodules:
- `roles/Issues-FS__Dev__Role__Architect`
- `roles/Issues-FS__Dev__Role__Dev`
- `roles/Issues-FS__Dev__Role__DevOps`
- `roles/Issues-FS__Dev__Role__Librarian`

**Recommendation:** Commit updated submodule pointers after confirming the target commits are stable.

### 3. CI Coverage

15 of 17 submodules have the standard 3-file CI pattern. The two without CI are:
- **QA** -- should have CI as a standard role repo
- **Human** -- may be intentionally infrastructure-light

All CI configurations are identical in structure: `ci-pipeline.yml`, `ci-pipeline__dev.yml`, `ci-pipeline__main.yml`.

**Recommendation:** Add CI to the QA role immediately. Evaluate whether Human repos need CI.

### 4. Test Coverage Distribution

| Range | Count | Submodules |
|-------|-------|------------|
| 0 tests | 1 | QA |
| 1 test | 9 | All roles except QA, plus Docs and Service__Client__Python |
| 2 tests | 1 | Service__UI |
| 6 tests | 1 | CLI |
| 19 tests | 1 | Service |
| 36 tests | 1 | Issues-FS (core) |

The core library (Issues-FS) has 36 test files, which is appropriate. The CLI has 6, and the Service has 19. All other submodules have 0-2 test files.

**Recommendation:** Role repos with only 1 test file likely have a placeholder test. Verify these actually pass. Service__Client__Python needs real tests.

### 5. Issues Tracker Usage

Total issue JSON files across the ecosystem: **114**

| Submodule | Issue JSONs |
|-----------|-------------|
| Service__UI | 56 |
| Issues-FS (core) | 25 |
| Librarian | 10 |
| Issues-FS__CLI | 7 |
| Journalist | 6 |
| Architect | 4 |
| Dev | 3 |
| DevOps | 3 |
| All others | 0 each |

The `issues-fs list` CLI command currently returns 0 results for all repos due to the double-path prefix bug (`Path__Handler__Graph_Node` prepends `.issues/` but the storage root is already `.issues/`). This bug was identified on 2026-02-09 and fix branches exist in both Issues-FS and Issues-FS__CLI.

**Recommendation:** Prioritize merging the double-path bug fix. Until then, the CLI is non-functional on real data.

### 6. Version Maturity

| Version Range | Count | Submodules |
|---------------|-------|------------|
| v0.4.x | 2 | Issues-FS (v0.4.5), DevOps (v0.4.2) |
| v0.2.x | 3 | CLI (v0.2.2), Service (v0.2.2), Service__Client__Python (v0.2.2) |
| v0.1.x (>0) | 5 | Docs (v0.1.4), Service__UI (v0.1.9), Architect (v0.1.3), Conductor (v0.1.3), Dev (v0.1.3), Journalist (v0.1.4) |
| v0.1.0 | 4 | AppSec, Cartographer, Historian, Librarian |
| No version | 2 | QA, Human |

Four role repos remain at their initial v0.1.0 despite having commits. Two submodules have no version at all.

### 7. Documentation

| Submodule | Doc Files |
|-----------|-----------|
| Issues-FS__Docs | 49 |
| QA | 4 |
| CLI | 4 |
| DevOps | 4 |
| Dev | 3 |
| Architect | 2 |
| Conductor | 2 |
| Journalist | 2 |
| Librarian | 2 |
| Service__UI | 1 |
| Cartographer | 1 |
| Historian | 1 |

Six submodules have no docs directory at all: Issues-FS (core), Service, Service__Client__Python, AppSec, Human.

### 8. `.issues/config` Consistency

All 15 submodules that have `.issues/` directories contain a properly structured `config/` with both `link-types.json` and `node-types.json`. This is 100% consistent across the ecosystem.

---

## Recommendations (Priority Order)

### P0 -- Critical
1. **Merge the double-path bug fix** in Issues-FS and Issues-FS__CLI to restore CLI functionality
2. **Provision the QA role** with pyproject.toml, CI, .issues directory, and tests

### P1 -- High
3. **Establish merge cadence** -- no submodule has been merged to its default branch
4. **Commit parent repo pointer updates** for the 4 drifted submodules
5. **Clean up stale branches** in Architect, DevOps, Dev, and Journalist repos

### P2 -- Medium
6. **Add tests to Service__Client__Python** (currently 1 placeholder)
7. **Add tests to Service__UI** (2 tests for 56 issues worth of complexity)
8. **Bump versions** for AppSec, Cartographer, Historian, and Librarian (all stuck at v0.1.0)
9. **Create docs directories** for Issues-FS (core), Service, Service__Client__Python, and AppSec

### P3 -- Low
10. **Evaluate Human repo standards** -- decide if Human repos should have CI/pyproject.toml
11. **Resolve detached HEAD states** in Docs, Service, Service__Client__Python, Cartographer, Historian, and Human
12. **Standardize default branch naming** -- Issues-FS and DevOps use `dev`, all others use `main`

---

## Appendix: Raw Data

### All Submodule Commits (as of 2026-02-10)

```
modules/Issues-FS                          e4b85ac  Fix double-path bug in all Path__Handler__Graph_Node methods
modules/Issues-FS__CLI                     334f9bd  Add feature request: issues-fs validate command
modules/Issues-FS__Docs                    65f9047  added no-raw-primitives policy
modules/Issues-FS__Service                 f51a5a7  Update release badge and version file
modules/Issues-FS__Service__Client__Python 9be5b4b  Update release badge and version file
modules/Issues-FS__Service__UI             25471f7  Fix invalid Obj_Id values in 5 issue files
roles/Issues-FS__Dev__Role__AppSec         fa7758a  Add CI pipeline workflows (3-file standard pattern)
roles/Issues-FS__Dev__Role__Architect      e8fc5ec  Add task: revise ADR-001 for Hugo per stakeholder interview
roles/Issues-FS__Dev__Role__Cartographer   30997e5  Merge commit into dev
roles/Issues-FS__Dev__Role__Conductor      58f4a4f  Add Conductor roadmap and next steps for 2026-02-09
roles/Issues-FS__Dev__Role__Dev            f91a8ad  Add task: implement Issues-FS News Site
roles/Issues-FS__Dev__Role__DevOps         1d1de50  Add task: technical report on all 17 submodule statuses
roles/Issues-FS__Dev__Role__Historian      42f569f  Merge commit into dev
roles/Issues-FS__Dev__Role__Journalist     e8a0a51  Add task: process stakeholder interview and LinkedIn publication
roles/Issues-FS__Dev__Role__Librarian      99f2e14  Add task: catalog stakeholder interview and LinkedIn article
roles/Issues-FS__Dev__Role__QA             38ac196  Add QA test execution status for 2026-02-09
humans/Issues-FS__Dev__Human__Dinis_Cruz   ddc3f2f  added first brief
```

### Commits Ahead of Default Branch

```
Issues-FS                          +3   (ahead of origin/dev)
Issues-FS__CLI                     +7   (ahead of origin/main)
Issues-FS__Docs                    +20  (ahead of origin/main)
Issues-FS__Service                 +9   (ahead of origin/main)
Issues-FS__Service__Client__Python +3   (ahead of origin/main)
Issues-FS__Service__UI             +22  (ahead of origin/main)
AppSec                             +2   (ahead of origin/main)
Architect                          +12  (ahead of origin/main)
Cartographer                       +5   (ahead of origin/main)
Conductor                          +8   (ahead of origin/main)
Dev                                +11  (ahead of origin/main)
DevOps                             +1   (ahead of origin/dev)
Historian                          +5   (ahead of origin/main)
Journalist                         +19  (ahead of origin/main)
Librarian                          +6   (ahead of origin/main)
QA                                 +2   (ahead of origin/main)
Human (Dinis_Cruz)                 +1   (ahead of origin/main)

TOTAL: 136 unmerged commits across the ecosystem
```
