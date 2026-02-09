# DevOps Guide: Branch Protection for AI-Assisted Development

**Securing GitHub repositories when using Claude and other AI coding agents**

---

## 1. Overview

When integrating AI coding agents (e.g., Anthropic's Claude) into your development workflow, the agent is typically granted write access to repositories via a GitHub App installation. This access is scoped at the **repository level** — GitHub does not provide branch-level restrictions at the App installation screen.

Without additional safeguards, an AI agent with write access can push commits directly to protected branches (`main`, `dev`, `release/*`) or merge its own pull requests without human review. This guide documents the recommended mitigation using **GitHub branch rulesets**.

---

## 2. Threat Model

| Risk | Description | Mitigation |
|------|-------------|------------|
| Direct push to `main`/`dev` | Agent pushes commits directly to protected branches, bypassing code review | Restrict updates |
| Self-merged pull request | Agent opens a PR and immediately merges it via the GitHub API without human approval | Require PR before merging + required approvals |
| Force push / history rewrite | Agent force-pushes to rewrite commit history on protected branches | Block force pushes |
| Branch deletion | Agent deletes `main` or `dev` branch via API | Restrict deletions |

---

## 3. Key Distinction: Push vs. Merge

A common misconception is that blocking direct pushes also prevents unapproved merges. GitHub treats these as **separate operations**:

| Operation | What It Does | Controlled By |
|-----------|-------------|---------------|
| `git push` | Directly updates a branch ref with new commits | **Restrict updates** |
| PR merge | Merges a pull request via the GitHub UI or API | **Require pull request before merging** |

**Both rules are required for complete coverage.** Without the PR requirement, an agent blocked from pushing directly can still open a PR and merge it immediately through the API.

---

## 4. Configuration: GitHub Branch Rulesets

We use **rulesets** (not classic branch protection rules) as they offer more granular control and are GitHub's recommended approach going forward.

### 4.1 Navigate to Settings

```
Repository → Settings → Code and automation → Branches → Add branch ruleset
```

### 4.2 Ruleset Configuration

**Basic Settings:**

| Setting | Value |
|---------|-------|
| Ruleset name | `protect-main-dev` |
| Enforcement status | **Active** |

**Bypass List:**

Add the roles that should retain direct push/merge access:

- `Organization admin` → Always allow
- `Repository admin` → Always allow

> **Note:** Only actors on the bypass list can skip the rules. All other users, GitHub Apps (including Claude), and bots must go through the PR approval workflow.

**Target Branches:**

Add the branches to protect:

- `main`
- `dev`

Optionally add patterns like `release/*` if applicable.

**Rules to Enable:**

| Rule | Purpose |
|------|---------|
| ☑ Restrict creations | Prevents non-bypass actors from creating matching branch names |
| ☑ Restrict updates | Blocks direct pushes to protected branches |
| ☑ Restrict deletions | Prevents branch deletion |
| ☑ Require a pull request before merging | Forces all changes through PR workflow |
| ↳ Required approvals: **1** (minimum) | Ensures a human reviews before merge |
| ☑ Block force pushes | Prevents history rewrites |

---

## 5. Resulting Workflow

After applying this ruleset, the development workflow with an AI agent looks like this:

**AI Agent (Claude):**

1. Creates a feature branch (e.g., `claude/fix-auth-bug`)
2. Pushes commits to the feature branch (unrestricted)
3. Opens a pull request targeting `main` or `dev`
4. **Cannot merge** — awaits human approval

**Human Reviewer (org/repo admin):**

1. Reviews the PR diff and CI results
2. Approves or requests changes
3. Merges when satisfied (bypass list grants direct merge rights)

**Human Developer (org/repo admin):**

1. Can push directly to `main`/`dev` if needed (bypass list)
2. Can merge PRs without requiring additional approval (bypass list)

---

## 6. Verification

After saving the ruleset, verify it's working:

```bash
# Clone the repo and attempt a direct push to main as a non-admin
git checkout main
echo "test" >> README.md
git commit -am "test direct push"
git push origin main
# Expected: rejected by branch ruleset
```

You can also verify via the GitHub API:

```bash
# List rulesets for a repository
curl -H "Authorization: Bearer $GITHUB_TOKEN" \
  https://api.github.com/repos/{owner}/{repo}/rulesets
```

---

## 7. Scope and Limitations

**What this protects:**

- `main` and `dev` branches from unauthorized pushes, merges, deletions, and force pushes
- Ensures all AI-generated code goes through human review before reaching protected branches

**What this does NOT control:**

- Which repositories the AI agent can access (controlled at the GitHub App installation level)
- What the agent does on feature branches (unrestricted by design)
- Repository-level settings like webhook creation, secret access, or Actions configuration (controlled by App permissions)

**GitHub Plan Requirements:**

- Branch rulesets are available on **Free** plans for public repos and **Team/Enterprise** for private repos
- Classic branch protection rules are an alternative on plans that don't support rulesets, but offer less granularity

---

## 8. Applying Across Multiple Repositories

If you manage multiple repositories with AI agent access (e.g., `Issues-FS`, `Issues-FS__Dev`, `Issues-FS__CLI`), this ruleset must be configured **per repository**. GitHub does not support organization-wide branch rulesets at the repository level.

For orgs managing many repos, consider automating ruleset creation via the GitHub API:

```bash
curl -X POST \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/repos/{owner}/{repo}/rulesets \
  -d '{
    "name": "protect-main-dev",
    "enforcement": "active",
    "target": "branch",
    "bypass_actors": [
      { "actor_id": 1, "actor_type": "OrganizationAdmin", "bypass_mode": "always" }
    ],
    "conditions": {
      "ref_name": {
        "include": ["refs/heads/main", "refs/heads/dev"],
        "exclude": []
      }
    },
    "rules": [
      { "type": "deletion" },
      { "type": "non_fast_forward" },
      { "type": "update" },
      { "type": "pull_request", "parameters": { "required_approving_review_count": 1 } }
    ]
  }'
```

---

## 9. Summary

Granting an AI coding agent repository access does not mean granting it unrestricted control over your critical branches. GitHub branch rulesets provide the necessary separation: the agent can create branches and open PRs freely, but a human must approve before anything lands on `main` or `dev`. This preserves the speed benefits of AI-assisted development while maintaining your existing code review and release governance.
