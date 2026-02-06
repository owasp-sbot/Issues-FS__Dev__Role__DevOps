# Runbook: Create a New Issues-FS Repo (No GitHub UI)

**Role:** DevOps

This runbook describes how to create a new Issues-FS repo, add an initial commit, and (optionally) add it as a submodule in `Issues-FS__Dev`.

## Inputs

- Org: `owasp-sbot`
- Repo name: `Issues-FS__Something`
- Default branch: `dev` (preferred) or `main`
- Visibility: public or private
- Submodule path (if applicable): `modules/<RepoName>` or `roles/<RepoName>`

## Step 1: Create the Repo

Using GitHub CLI (`gh`):

```bash
gh repo create owasp-sbot/<RepoName> --public --confirm
```

For private:

```bash
gh repo create owasp-sbot/<RepoName> --private --confirm
```

## Step 2: Add Initial Commit (Fixes "branch yet to be born")

If the repo is empty, you must push an initial commit before adding it as a submodule.

```bash
mkdir /tmp/<RepoName> && cd /tmp/<RepoName>
git init
git checkout -b dev

echo "# <RepoName>" > README.md
git add README.md
git commit -m "Initial commit"

git remote add origin git@github.com:owasp-sbot/<RepoName>.git
git push -u origin dev
```

If you prefer `main`, switch `dev` to `main` above.

## Step 3: Add as Submodule in Issues-FS__Dev

```bash
cd /Users/diniscruz/_dev/owasp-sbot/Issues-FS__Dev

git submodule add -b dev git@github.com:owasp-sbot/<RepoName>.git <submodule-path>
```

Example:

```bash
git submodule add -b dev git@github.com:owasp-sbot/Issues-FS__CLI.git modules/Issues-FS__CLI
```

## Step 4: Sync Submodules (if needed)

```bash
git submodule sync --recursive
git submodule update --init --recursive
```

## Troubleshooting

**Error:** `fatal: You are on a branch yet to be born`

- Cause: repo has no default branch or no commits yet.
- Fix: perform Step 2 to push the initial commit.

**Error:** `untracked working tree files would be overwritten by checkout`

- Cause: local untracked files in the submodule path.
- Fix: commit or move the files, then rerun `git submodule update --init --recursive`.
