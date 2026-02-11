# Post-Merge Tag/Commit Mapping Report

**Date:** 2026-02-11
**Context:** All feature branches across all 17 submodules were merged to their default branches (dev or main), branches deleted, and ecosystem consolidated. This report captures the state of every repo immediately after that merge.

---

## Tag/Commit Mapping

| Repo | Default Branch | Current Tag | Commit SHA | Commit Message | Date |
|------|---------------|-------------|------------|----------------|------|
| **Issues-FS__Dev** (parent) | dev | v0.2.14 | `11cc8a6d` | Update release badge and version file | 2026-02-11 00:15:16 UTC |
| **Issues-FS** | dev | v0.5.1 | `bd0a1ded` | Update release badge and version file | 2026-02-10 22:51:46 UTC |
| **Issues-FS__Service__Client__Python** | main | v0.2.2 | `9be5b4be` | Update release badge and version file | 2026-02-06 17:50:41 UTC |
| **Issues-FS__Service** | main | v0.2.2 | `f51a5a70` | Update release badge and version file | 2026-02-06 17:49:52 UTC |
| **Issues-FS__Service__UI** | main | v0.2.1 | `05a6644a` | Update release badge and version file | 2026-02-11 00:03:57 UTC |
| **Issues-FS__Docs** | main | v0.1.5 | `b0181ce1` | Update release badge and version file | 2026-02-09 19:58:21 UTC |
| **Issues-FS__CLI** | main | v0.3.1 | `177758d7` | Update release badge and version file | 2026-02-10 03:37:31 UTC |
| **Issues-FS__Dev__Role__DevOps** | dev | v0.1.1 | `bb32b83b` | Update release badge and version file | 2026-02-10 23:32:38 UTC |
| **Issues-FS__Dev__Role__Librarian** | main | v0.2.1 | `ac043124` | Update release badge and version file | 2026-02-11 00:03:06 UTC |
| **Issues-FS__Dev__Role__Dev** | main | v0.1.7 | `d8a023af` | Update release badge and version file | 2026-02-10 23:31:16 UTC |
| **Issues-FS__Dev__Role__Conductor** | main | v0.1.6 | `bedb1883` | Update release badge and version file | 2026-02-10 23:18:42 UTC |
| **Issues-FS__Dev__Role__Architect** | main | v0.1.4 | `addfc3b9` | Update release badge and version file | 2026-02-10 23:17:31 UTC |
| **Issues-FS__Dev__Role__Cartographer** | main | v0.1.2 | `dcfd5fd9` | Update release badge and version file | 2026-02-10 02:15:31 UTC |
| **Issues-FS__Dev__Role__AppSec** | main | v0.1.2 | `ed2fc21d` | Update release badge and version file | 2026-02-10 22:57:30 UTC |
| **Issues-FS__Dev__Role__Historian** | main | v0.1.1 | `bafc4ad2` | Update release badge and version file | 2026-02-10 23:35:06 UTC |
| **Issues-FS__Dev__Role__Journalist** | main | v0.2.1 | `ed4a84d3` | Update release badge and version file | 2026-02-11 00:02:42 UTC |
| **Issues-FS__Dev__Role__QA** | main | v0.1.1 | `4e196868` | Update release badge and version file | 2026-02-10 23:59:33 UTC |
| **Issues-FS__Dev__Human__Dinis_Cruz** | main | *(none)* | `ddc3f2f5` | added first brief | 2026-02-09 20:32:07 UTC |

---

## Summary

- **18 repos total** (1 parent + 6 modules + 10 roles + 1 human)
- **17 of 18** repos have semver tags; the Human repo (Dinis_Cruz) has no tags
- **All repos clean**: No uncommitted changes in any repo
- **All HEAD commits** are "Update release badge and version file" (except Dinis_Cruz: "added first brief")
- **2 repos** default to `dev` (Issues-FS, DevOps); **15 repos** default to `main`; **parent** is on `dev`

---

## Issues Found

### 1. Leftover Remote Branches (12 branches across 6 repos)

| Repo | Leftover Branch |
|------|----------------|
| Issues-FS__Dev (parent) | `origin/claude/check-gh-submodule-token-rcalB` |
| Issues-FS__Dev (parent) | `origin/claude/review-submodules-zzzOI` |
| Issues-FS__Dev__Role__DevOps | `origin/claude/add-submodule-report-task-rcalB` |
| Issues-FS__Dev__Role__Dev | `origin/claude/add-debrief-rcalB` |
| Issues-FS__Dev__Role__Dev | `origin/claude/add-news-site-impl-task-rcalB` |
| Issues-FS__Dev__Role__Dev | `origin/claude/add-report-conventions-QaKVH` |
| Issues-FS__Dev__Role__Dev | `origin/claude/add-status-reports` |
| Issues-FS__Dev__Role__Architect | `origin/claude/add-news-site-adr-task-rcalB` |
| Issues-FS__Dev__Role__Journalist | `origin/claude/add-article-task-rcalB` |
| Issues-FS__Dev__Role__Journalist | `origin/claude/add-news-site-adr-task-rcalB` |
| Issues-FS__Dev__Role__Journalist | `origin/claude/add-role-setup` |
| Issues-FS__Dev__Role__Journalist | `origin/claude/add-status-reports` |
| Issues-FS__Dev__Role__QA | `origin/claude/add-status-reports` |
| Issues-FS__Dev__Human__Dinis_Cruz | `origin/claude/add-ci-infra-rcalB` |

### 2. .gitmodules Branch Mismatch

All 17 submodules are configured with `branch = dev` in `.gitmodules`, but 12 of 17 have had their `dev` branch deleted after the merge (only `main` remains on the remote). This means `git submodule update --remote` will fail for those repos because the configured tracking branch no longer exists.

**Affected repos** (configured as `branch = dev` but only `main` exists on remote):
- modules/Issues-FS__Service__Client__Python
- modules/Issues-FS__Service
- modules/Issues-FS__Service__UI
- modules/Issues-FS__Docs
- modules/Issues-FS__CLI
- roles/Issues-FS__Dev__Role__Librarian
- roles/Issues-FS__Dev__Role__Dev
- roles/Issues-FS__Dev__Role__Conductor
- roles/Issues-FS__Dev__Role__Architect
- roles/Issues-FS__Dev__Role__Cartographer
- roles/Issues-FS__Dev__Role__AppSec
- roles/Issues-FS__Dev__Role__Historian

### 3. Inconsistent Default Branch Strategy

After the merge, the ecosystem has an inconsistent branch layout:
- **2 repos** have only `dev` on the remote (Issues-FS, DevOps) -- no `main` branch
- **12 repos** have only `main` on the remote -- `dev` was deleted after merge
- **3 repos** have both `dev` and `main` (Journalist, QA, Dinis_Cruz)
- **Parent repo** has both `dev` and `main`

### 4. No Tags on Human Repo

`Issues-FS__Dev__Human__Dinis_Cruz` has no semver tags. All other repos have proper versioned tags.

### 5. Local Branches From Other Agents

Three submodules are not in detached HEAD state -- they are on local branches created by other agents during this session:
- Conductor: `claude/post-merge-planning-rcalB`
- Historian: `claude/post-merge-milestone-rcalB`
- Journalist: `claude/post-merge-coverage-rcalB`

This is expected behavior (other agents working in parallel) but is noted for completeness.

---

## Recommendations

1. **Delete leftover remote branches** -- All 12 `claude/*` branches listed above should be deleted via the GitHub REST API (since `git push --delete` is blocked by the proxy).
2. **Update `.gitmodules`** -- Change `branch = dev` to `branch = main` for the 12 repos that no longer have a `dev` branch on the remote. Alternatively, recreate `dev` branches from `main` if the dev/main branching strategy is intended to continue.
3. **Decide on branch strategy** -- The ecosystem needs a consistent post-merge branch layout. Either all repos should have `dev` + `main`, or all should be `main`-only. The current mixed state will cause operational issues.
4. **Add CI infrastructure to Human repo** -- The Dinis_Cruz human repo has no tags and no CI. Consider whether it needs the standard CI pattern or is intentionally minimal.
