# GitHub — SDET Quick Cheatsheet

## Core

| Keyword    | Remember                    |
| ---------- | --------------------------- |
| GitHub     | Git hosting + collaboration |
| Repository | Project hosted on GitHub    |
| Fork       | Your copy of another repo   |
| Clone      | GitHub → Local              |
| PR         | Request to merge changes    |
| Issue      | Track work/bugs             |
| Actions    | CI/CD automation            |
| Secrets    | Secure credentials          |
| Release    | Published version           |

## Repository

**Visibility**

* Public
* Private
* Internal

**README.md** → Project documentation

## Fork vs Clone

* Fork → GitHub → Your GitHub account
* Clone → GitHub → Local machine

## Origin vs Upstream

```text
origin   → Your remote
upstream → Original repository
```

Common open-source flow:

```text
upstream → fork → clone → branch → PR
```

## Pull Request

```text
Branch
  ↓
Push
  ↓
PR
  ↓
Review
  ↓
CI Checks
  ↓
Approval
  ↓
Merge
```

**PR**

* Title
* Description
* Reviewers
* Checks
* Approvals
* Conversations

## PR Types

* Open PR
* Draft PR
* Approved
* Changes requested
* Merged
* Closed

## Merge Strategies

| Strategy     | Keyword                 |
| ------------ | ----------------------- |
| Merge commit | Preserve branch history |
| Squash       | One commit              |
| Rebase       | Linear history          |

## Code Review

Check:

* Logic
* Test coverage
* Naming
* Maintainability
* Security
* Flaky tests
* CI results

## Issues

**Issue →** Bug / Task / Enhancement

Useful:

* Labels
* Assignee
* Milestone
* Comments

## GitHub Actions

**Workflow → Event → Job → Step**

Example:

```text
Push / PR
   ↓
GitHub Actions
   ↓
Checkout
   ↓
Setup Java
   ↓
Run Tests
   ↓
Publish Report
```

Common files:

```text
.github/workflows/test.yml
```

Common events:

```yaml
push
pull_request
workflow_dispatch
schedule
```

## GitHub Secrets

**Use for:**

* API keys
* Tokens
* Passwords
* Cloud credentials

```text
Never hardcode secrets in code.
```

## Authentication

### HTTPS

```text
GitHub + Personal Access Token
```

### SSH

```text
SSH Key → GitHub
```

**Q:** SSH vs HTTPS?

## Branch Protection

Common rules:

* PR required
* Approval required
* CI checks required
* No direct push
* No force push

## Releases

```text
Tag → Release → Version
```

Example:

```text
v1.0.0
v1.1.0
v2.0.0
```

## SDET GitHub Workflow

```text
Clone
 ↓
Create branch
 ↓
Write automation
 ↓
Commit
 ↓
Push
 ↓
PR
 ↓
GitHub Actions
 ↓
Code Review
 ↓
Merge
```

## Must-Know Questions

* Git vs GitHub?
* Fork vs Clone?
* Origin vs Upstream?
* What is a PR?
* Draft PR?
* Merge vs Squash vs Rebase?
* What is GitHub Actions?
* Workflow vs Job vs Step?
* What triggers a workflow?
* How do you run automation through GitHub Actions?
* Where should API keys be stored?
* What are GitHub Secrets?
* SSH vs HTTPS?
* What is branch protection?
* What is a GitHub Release?
* How do you implement CI for Selenium/API tests?
* How do you prevent direct pushes to `main`?


********************************************************************************************


# Git — SDET Quick Cheatsheet

## Core

| Keyword      | Remember                |
| ------------ | ----------------------- |
| Git          | Version Control         |
| Repository   | Project + history       |
| Working Tree | Current files           |
| Staging      | Changes ready to commit |
| Commit       | Saved snapshot          |
| HEAD         | Current position        |
| Branch       | Isolated development    |
| Remote       | Remote repository       |

```text
Working → add → Staging → commit → Local → push → Remote
```

## Setup

```bash
git config --global user.name "Name"
git config --global user.email "Email"
git init
git clone <url>
git status
```

## Stage / Commit

```bash
git add <file>
git add .
git add -A
git commit -m "message"
git commit --amend
```

## Inspect

```bash
git status
git diff
git diff --staged
git log --oneline
git log --oneline --graph --all
```

## Branch

```bash
git branch
git switch <branch>
git switch -c <branch>
git branch -d <branch>
```

**Flow:** `main → feature → PR → merge`

## Merge / Rebase

```bash
git merge <branch>
git rebase <branch>
git rebase --continue
git rebase --abort
```

* Merge → combine histories
* Rebase → replay commits / linear history
* Shared commits → avoid rebase

## Conflict

```bash
git status
# resolve
git add .
git commit
```

## Stash

```bash
git stash
git stash list
git stash apply
git stash pop
git stash drop
```

**Stash = temporary uncommitted work**

## Remote

```bash
git remote -v
git fetch
git pull
git push
git push -u origin <branch>
```

| Command | Meaning           |
| ------- | ----------------- |
| Fetch   | Download changes  |
| Pull    | Fetch + integrate |
| Push    | Upload commits    |

## Undo

```bash
git restore <file>
git restore --staged <file>

git reset --soft HEAD~1
git reset HEAD~1
git reset --hard HEAD~1

git revert <commit>
```

* Restore → discard/unstage
* Reset → move history
* Revert → new undo commit
* Shared branch → prefer `revert`

## Recovery / Advanced

```bash
git reflog
git cherry-pick <commit>
git bisect start
git tag v1.0.0
```

* Reflog → recover lost commits
* Cherry-pick → specific commit
* Bisect → find regression commit
* Tag → version/release marker

## .gitignore

```gitignore
*.log
.env
target/
.idea/
allure-results/
```

```bash
git rm --cached <file>
```

**Important:** `.gitignore` doesn't untrack existing files.

## SDET Daily Flow

```text
pull → branch → code → test → status/diff
→ add → commit → push → PR → CI → merge
```

## Must-Know Questions

* Git vs GitHub?
* `add` vs `commit`?
* Merge vs Rebase?
* Fetch vs Pull?
* Reset vs Revert?
* Soft vs Mixed vs Hard reset?
* HEAD?
* Detached HEAD?
* Stash?
* Reflog?
* Cherry-pick?
* Bisect?
* Tag vs Branch?
* How to resolve conflict?
* How to recover lost commit?
* How to undo pushed commit?
* How to remove tracked file but keep locally?
