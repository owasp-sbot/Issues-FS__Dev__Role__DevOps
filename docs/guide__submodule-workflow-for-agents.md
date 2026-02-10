# Guide: Submodule Workflow for AI Agents

**Document identifier:** guide__submodule-workflow-for-agents
**Version:** v0.1.0
**Date:** 2026-02-10
**Status:** Active

---

## Submodule State After Init

After running `git submodule update`, every submodule is in **detached HEAD** state. This means:

- The working tree has files, but there is no branch
- Any commits made will be orphaned when switching away
- Files written to the filesystem may appear to exist but will not survive a `git checkout`

## Required Workflow: Branch Before Write

When making changes inside a submodule, always follow this order:

```bash
cd roles/Issues-FS__Dev__Role__<Name>
git checkout -b claude/<description>-<sessionSuffix>
# NOW write files, make changes
git add <files>
git commit -m "Description"
git push -u origin claude/<description>-<sessionSuffix>
```

**Critical**: Create the branch BEFORE writing any files. Files written while in detached HEAD state will not be associated with the new branch.

## Pushing From Submodules

- Each `git push` must be run from **inside** the submodule directory
- Use `git push -u origin <branch>` to set up tracking
- The `GH_SUBMODULE_TOKEN` URL rewrite handles authentication automatically
- Branch names must follow pattern: `claude/<description>-<sessionSuffix>`

## Branch Protection

- `main` and `dev` branches are protected - cannot push directly
- Always create feature branches and open PRs
- Use the `scripts/gh-release-to-main.sh` script for releases

## Common Pitfalls

### Files disappear after branch creation
**Cause**: Files were written while HEAD was detached, then `git checkout -b` was run.
**Fix**: Always create the branch first, then write files.

### Push fails with "does not match any"
**Cause**: Running `git push` from the wrong directory (parent repo instead of submodule).
**Fix**: `cd` into the submodule directory before pushing.

### Push fails with 403
**Cause**: Branch name doesn't match the expected pattern, or pushing to a protected branch.
**Fix**: Ensure branch starts with `claude/` and ends with the session suffix.

### Recovering lost content
If content was committed to the parent repo but needs to be in a submodule:
```bash
git show <commit-hash>:<file-path> > /path/in/submodule/filename
```

## Background Agent Considerations

When using background agents that write to submodules:

- Background agents can time out silently without producing output files
- For critical tasks, prefer running agents in the foreground
- Always verify output files exist before assuming success
- If an agent needs to write to a submodule, ensure the submodule branch is created before launching the agent
