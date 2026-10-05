# Git — SDET Cheatsheet

> Git concepts and commands required for an SDET / QA Automation engineer.

---

# 1. What is Git?

Git is a **Distributed Version Control System (DVCS)** used to track changes in source code and collaborate with other developers.

### Why SDETs use Git

As an SDET, Git is commonly used to:

* Store automation code
* Create feature branches
* Commit test changes
* Collaborate with developers and testers
* Review automation code
* Maintain different versions of test frameworks
* Integrate automation with CI/CD

### Simple Definition

```text
Git = Version Control System
```

---

# 2. Git vs GitHub

| Git                    | GitHub                                |
| ---------------------- | ------------------------------------- |
| Version Control System | Platform for hosting Git repositories |
| Runs locally           | Cloud-based                           |
| Tracks code changes    | Provides collaboration features       |
| `git commit`           | Pull Requests                         |
| `git branch`           | Code Review                           |
| `git merge`            | GitHub Actions                        |

```text
Git
↓
Version Control

GitHub
↓
Hosting + Collaboration + CI/CD
```

---

# 3. Git Architecture

```text
Working Directory
       │
       │ git add
       ↓
Staging Area
       │
       │ git commit
       ↓
Local Repository
       │
       │ git push
       ↓
Remote Repository
(GitHub)
```

## Working Directory

The files you are currently working on.

## Staging Area

The changes selected for the next commit.

## Local Repository

Contains commits and Git history on your computer.

## Remote Repository

Repository hosted on GitHub or another Git server.

---

# 4. `git init`

Creates a new local Git repository.

```bash
git init
```

Creates:

```text
.git/
```

### Important

`git init` does **not**:

* Create a GitHub repository
* Upload code
* Create a commit

---

# 5. `git clone`

Creates a local copy of a remote repository.

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/user/sdet-project.git
```

Typical workflow:

```text
GitHub Repository
       ↓
   git clone
       ↓
Local Repository
```

---

# 6. `git status`

Shows the current state of the repository.

```bash
git status
```

It can show:

* Modified files
* Untracked files
* Deleted files
* Staged files
* Current branch

### Best Practice

Run:

```bash
git status
```

before and after important Git operations.

---

# 7. `git add`

Moves changes into the staging area.

## Single file

```bash
git add LoginTest.java
```

## Multiple files

```bash
git add LoginTest.java PaymentTest.java
```

## All changes

```bash
git add .
```

or:

```bash
git add -A
```

### Flow

```text
Working Directory
       ↓
    git add
       ↓
Staging Area
```

### `git add .` vs `git add -A`

```text
git add .
→ Stages changes under the current directory

git add -A
→ Stages all changes in the repository
```

---

# 8. `git commit`

Creates a snapshot of staged changes.

## Single file

```bash
git add LoginTest.java
git commit -m "Add login automation"
```

## Multiple files

```bash
git add LoginTest.java PaymentTest.java
git commit -m "Add login and payment tests"
```

## All changes

```bash
git add .
git commit -m "Update automation tests"
```

### Good commit message

```bash
git commit -m "Add API login validation tests"
```

Avoid:

```bash
git commit -m "changes"
git commit -m "update"
```

### Important

Only **staged changes** are included in a normal commit.

---

# 9. `git diff`

Shows changes between versions.

## Unstaged changes

```bash
git diff
```

Shows changes in the working directory that have **not** been staged.

## Staged changes

```bash
git diff --staged
```

Shows changes that are already staged for commit.

## All changes compared with HEAD

```bash
git diff HEAD
```

### Remember

```text
git diff
↓
Unstaged changes

git diff --staged
↓
Staged changes
```

### SDET Practice

Before committing automation:

```bash
git status
git diff
git diff --staged
git commit -m "Add login automation"
```

---

# 10. Git Branch

A branch is an independent line of development.

Example:

```text
             feature-login
            /
main ──────●──────
            \
             feature-payment
```

Branches allow developers and SDETs to work on features without directly modifying `main`.

---

## List branches

```bash
git branch
```

## Create branch

```bash
git branch feature-login
```

## Switch branch

```bash
git switch feature-login
```

## Create and switch

```bash
git switch -c feature-login
```

## Delete branch

```bash
git branch -d feature-login
```

Force delete:

```bash
git branch -D feature-login
```

---

# 11. Typical SDET Branch Workflow

```text
main
 ↓
feature-login
 ↓
Write automation
 ↓
Commit
 ↓
Push
 ↓
Pull Request
 ↓
Code Review
 ↓
Merge
 ↓
main
```

### Example

```bash
git switch main
git pull

git switch -c feature-login

# Write automation

git add .
git commit -m "Add login automation"

git push -u origin feature-login
```

---

# 12. Git Merge

Combines changes from one branch into another.

```bash
git switch main
git merge feature-login
```

Example:

```text
Before:

main       A ── B

feature         └── C ── D


After:

main       A ── B ── C ── D
```

### Why SDETs need merge

You may need to merge:

* Automation features
* Bug fixes
* Framework changes
* Test improvements

---

# 13. Merge Conflict

A merge conflict occurs when Git cannot automatically combine changes.

Example:

```text
<<<<<<< HEAD
Your changes
=======
Other branch changes
>>>>>>> feature-login
```

## How to resolve

```text
1. Open the conflicting file
2. Understand both changes
3. Decide what to keep
4. Remove conflict markers
5. Save the file
6. Stage the file
7. Complete the merge
```

Commands:

```bash
git add .
git commit
```

### Important

Merge conflicts are normal when multiple developers modify the same code.

---

# 14. Git Rebase

Rebase moves/replays your commits onto another base.

Before:

```text
A ── B ── C        main
     \
      D ── E       feature
```

After:

```text
A ── B ── C ── D' ── E'
```

Command:

```bash
git switch feature
git rebase main
```

### Why use rebase?

To keep your feature branch updated with the latest `main` and maintain a cleaner history.

### Merge vs Rebase

| Merge                             | Rebase                          |
| --------------------------------- | ------------------------------- |
| Combines histories                | Replays commits                 |
| May create merge commit           | Usually creates linear history  |
| Does not rewrite existing commits | Rewrites commit identities      |
| Safer for shared branches         | Be careful with shared branches |

### Golden Rule

> Avoid rebasing commits that other people are already using.

---

# 15. Git Stash

Temporarily stores uncommitted changes.

### Example scenario

You are working on:

```text
feature-login
```

Your changes are incomplete.

Suddenly, you need to fix an urgent bug on another branch.

Instead of committing incomplete work:

```bash
git stash
```

Switch branch:

```bash
git switch bug-fix
```

After fixing the bug:

```bash
git switch feature-login
git stash pop
```

---

## Useful commands

### Stash

```bash
git stash
```

### Stash with message

```bash
git stash push -m "Login automation WIP"
```

### List stashes

```bash
git stash list
```

### Apply stash

```bash
git stash apply
```

### Apply and remove stash

```bash
git stash pop
```

### Delete stash

```bash
git stash drop
```

### Delete all stashes

```bash
git stash clear
```

---

# 16. Git Remote

A remote represents another repository, usually on GitHub.

## List remotes

```bash
git remote
```

## Detailed information

```bash
git remote -v
```

Example:

```text
origin  https://github.com/user/project.git
```

## Add remote

```bash
git remote add origin <repository-url>
```

## Change remote URL

```bash
git remote set-url origin <new-url>
```

### What is `origin`?

`origin` is the conventional name given to the remote repository when you clone a repository.

```text
Local Repository
      │
      │ origin
      ↓
GitHub Repository
```

> `origin` is only a conventional name. It is not a special Git keyword.

---

# 17. `git fetch`

Downloads changes from the remote repository without automatically integrating them into your current branch.

```bash
git fetch
```

or:

```bash
git fetch origin
```

Conceptually:

```text
GitHub
  ↓
git fetch
  ↓
Local remote-tracking information
```

Your current branch is not automatically changed.

### Useful before updating your branch

```bash
git fetch
git log
```

---

# 18. `git pull`

Downloads remote changes and integrates them into your current branch.

```bash
git pull
```

Example:

```bash
git pull origin main
```

Conceptually:

```text
git pull
=
git fetch
+
integration
```

The integration may be performed using merge or rebase depending on configuration/options.

---

# 19. `git push`

Uploads local commits to the remote repository.

```bash
git push
```

For a new branch:

```bash
git push -u origin feature-login
```

After setting the upstream:

```bash
git push
```

is usually enough.

### Flow

```text
Local Repository
       ↓
   git push
       ↓
GitHub Repository
```

---

# 20. Fetch vs Pull vs Push

| Command     | Direction      | Purpose                     |
| ----------- | -------------- | --------------------------- |
| `git fetch` | Remote → Local | Download remote information |
| `git pull`  | Remote → Local | Fetch + integrate           |
| `git push`  | Local → Remote | Upload commits              |

### Easy memory trick

```text
FETCH
↓
Get information

PULL
↓
Get + integrate

PUSH
↓
Send changes
```

---

# 21. Git Log

Used to view commit history.

## Full history

```bash
git log
```

## One-line history

```bash
git log --oneline
```

Example:

```text
a123456 Add payment tests
b234567 Add login tests
c345678 Initial commit
```

## Graph

```bash
git log --oneline --graph --all
```

Useful for understanding branches and merges.

---

# 22. HEAD

`HEAD` represents your current location in Git history.

Normally:

```text
HEAD
 ↓
main
 ↓
Latest commit
```

For example:

```bash
git log --oneline
```

```text
a123456 Add payment tests
b234567 Add login tests
c345678 Initial commit
```

Normally `HEAD` points to the latest commit of your current branch.

---

# 23. Detached HEAD

A detached HEAD occurs when `HEAD` points directly to a commit instead of a branch.

Example:

```bash
git checkout <commit-id>
```

or:

```bash
git switch --detach <commit-id>
```

Now:

```text
HEAD
 ↓
Commit
```

instead of:

```text
HEAD
 ↓
Branch
 ↓
Commit
```

### Return to a branch

```bash
git switch main
```

### Keep work created from detached HEAD

```bash
git switch -c recovery-branch
```

---

# 24. `git restore`

Used to restore files and staging state.

## Discard unstaged changes

```bash
git restore LoginTest.java
```

This restores the file to its staged/HEAD state depending on what is available.

## Unstage a file

```bash
git restore --staged LoginTest.java
```

This removes the file from the staging area while keeping the working-directory changes.

### Remember

```text
git restore file
↓
Restore working-directory file

git restore --staged file
↓
Unstage file
```

> Be careful: restoring a file can discard uncommitted changes.

---

# 25. `git reset`

Moves the current branch to another commit.

## Soft reset

```bash
git reset --soft HEAD~1
```

Removes the commit but keeps the changes staged.

```text
Commit removed
Changes → Staged
```

## Mixed reset

```bash
git reset HEAD~1
```

Removes the commit and unstages the changes.

```text
Commit removed
Changes → Working Directory
```

## Hard reset

```bash
git reset --hard HEAD~1
```

Removes the commit and discards associated uncommitted changes.

```text
Commit removed
Changes → Discarded
```

> ⚠️ Be very careful with `git reset --hard`.

---

# 26. `git revert`

Creates a new commit that reverses an earlier commit.

```bash
git revert <commit-id>
```

Example:

```text
A → B → C

Revert B

A → B → C → D
```

`D` contains the changes that undo `B`.

### When to use?

For shared branches such as `main`, `revert` is generally safer than rewriting history.

---

# 27. Reset vs Revert

| Reset                 | Revert                    |
| --------------------- | ------------------------- |
| Moves branch/HEAD     | Creates a new commit      |
| Can rewrite history   | Preserves history         |
| Useful for local work | Safer for shared branches |
| `git reset`           | `git revert`              |

### Easy rule

```text
Private/local work
→ reset can be useful

Shared/public history
→ prefer revert
```

---

# 28. Git Reflog

`git reflog` records movements of `HEAD` and branch references.

```bash
git reflog
```

Useful for recovering from:

* Accidental reset
* Incorrect rebase
* Lost commits
* Accidentally moved branch references

Example:

```text
HEAD@{0}
HEAD@{1}
HEAD@{2}
```

### Example recovery scenario

You accidentally execute:

```bash
git reset --hard HEAD~3
```

Use:

```bash
git reflog
```

Find the previous commit and recover using an appropriate Git command.

---

# 29. Git Cherry-Pick

Applies a specific commit from another branch to your current branch.

Example:

```text
feature:
A → B → C → D

main:
A → B
```

You only need commit `C`.

```bash
git switch main
git cherry-pick <C-commit-id>
```

Result:

```text
main:
A → B → C'
```

### SDET use case

Suppose a critical automation bug fix exists on another branch, but you don't want to merge the entire branch.

You can cherry-pick only the required commit.

---

# 30. Git Bisect

Used to identify which commit introduced a bug.

Suppose:

```text
A → B → C → D → E
```

You know:

```text
A = Good
E = Bad
```

Start:

```bash
git bisect start
git bisect bad
git bisect good <commit-id>
```

Git checks out a middle commit.

Test it.

Then mark:

```bash
git bisect good
```

or:

```bash
git bisect bad
```

Continue until Git identifies the problematic commit.

Finish:

```bash
git bisect reset
```

### SDET use case

A regression test started failing somewhere among many commits.

`git bisect` helps find the commit that introduced the regression efficiently.

---

# 31. Git Tag

A tag is a reference to a specific commit.

Commonly used to mark releases.

```text
v1.0.0
v1.1.0
v2.0.0
```

## Create tag

```bash
git tag v1.0.0
```

## Annotated tag

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

## List tags

```bash
git tag
```

## Show tag

```bash
git show v1.0.0
```

## Push tag

```bash
git push origin v1.0.0
```

---

# 32. `.gitignore`

`.gitignore` specifies files that Git should not track.

### Common SDET examples

```text
target/
*.class
*.log
.env
.idea/
.vscode/
test-output/
allure-results/
```

### Why?

Avoid committing:

* Build files
* Logs
* IDE files
* Test reports
* Environment files
* Credentials
* API keys
* Temporary files

### Important

`.gitignore` does **not** automatically stop tracking a file that is already tracked.

Use:

```bash
git rm --cached <file>
```

if necessary.

---

# 33. Removing Files from Git

## Delete locally and from Git

```bash
git rm file.txt
```

## Remove from Git but keep locally

```bash
git rm --cached file.txt
```

### Common SDET scenario

You accidentally commit:

```text
config.properties
```

containing environment-specific data.

After adding it to `.gitignore`:

```bash
git rm --cached config.properties
```

Then commit the change.

> If sensitive information was already pushed, simply removing it from the latest commit is not always enough. Treat the credential as exposed and rotate/revoke it.

---

# 34. Typical SDET Git Workflow

```text
Clone Repository
       ↓
Create Feature Branch
       ↓
Write Automation
       ↓
git status
       ↓
git diff
       ↓
git add
       ↓
git diff --staged
       ↓
git commit
       ↓
git fetch / git pull
       ↓
git push
       ↓
Pull Request
       ↓
Code Review
       ↓
CI/CD
       ↓
Merge
```

### Practical example

```bash
git clone <repository-url>

cd sdet-project

git switch main
git pull

git switch -c feature-login

# Write automation

git status
git diff

git add .
git diff --staged

git commit -m "Add login automation"

git push -u origin feature-login
```

Then create a Pull Request on GitHub.

---

# 35. SDET Git Daily Workflow

When starting work:

```bash
git switch main
git pull
git switch -c feature-name
```

While working:

```bash
git status
git diff
```

Before commit:

```bash
git add .
git diff --staged
```

Commit:

```bash
git commit -m "Add login API tests"
```

Push:

```bash
git push -u origin feature-name
```

---

# 36. Most Important Commands

## Repository

```bash
git init
git clone
```

## Daily Work

```bash
git status
git add
git commit
git diff
```

## Branching

```bash
git branch
git switch
git merge
git rebase
```

## Remote

```bash
git remote
git fetch
git pull
git push
```

## Temporary Work

```bash
git stash
```

## History

```bash
git log
git show
```

## Undo / Recovery

```bash
git restore
git reset
git revert
git reflog
```

## Advanced but Important

```bash
git cherry-pick
git bisect
git tag
```

---

# 37. Golden Rules

1. Always check `git status`.
2. Review changes using `git diff`.
3. Review staged changes using `git diff --staged`.
4. Write meaningful commit messages.
5. Use feature branches.
6. Keep commits small and meaningful.
7. Never blindly use `git reset --hard`.
8. Be careful when rebasing shared branches.
9. Use `git revert` for safely undoing shared history.
10. Never commit passwords, API keys, tokens, or secrets.
11. Use `.gitignore`.
12. Pull/fetch the latest changes before starting significant work.

---

# 38. One-Minute Revision

```text
Git
↓
Version Control System

Repository
↓
Project tracked by Git

Working Directory
↓
Files being modified

Staging Area
↓
Changes selected for commit

Commit
↓
Snapshot of staged changes

Branch
↓
Independent line of development

Merge
↓
Combine branches

Rebase
↓
Replay commits onto another base

Stash
↓
Temporarily store uncommitted changes

Fetch
↓
Download remote information

Pull
↓
Fetch + integrate

Push
↓
Upload commits

HEAD
↓
Current location in Git history

Reset
↓
Move branch/HEAD

Revert
↓
Create undo commit

Restore
↓
Restore/unstage files

Reflog
↓
Recover previous references

Cherry-pick
↓
Apply one specific commit

Bisect
↓
Find commit that introduced a bug

Tag
↓
Mark a commit/release

.gitignore
↓
Ignore files from tracking
```

---

# 39. SDET Interview Quick Revision

### What is Git?

Git is a distributed version control system used to track and manage changes.

### What is GitHub?

GitHub is a platform for hosting Git repositories and providing collaboration and CI/CD features.

### What is a staging area?

An intermediate area where changes are selected before creating a commit.

### What is a branch?

An independent line of development.

### Merge vs Rebase?

```text
Merge
→ Combines histories

Rebase
→ Replays commits onto another base
```

### Fetch vs Pull?

```text
Fetch
→ Downloads remote information

Pull
→ Fetch + integrate changes
```

### Reset vs Revert?

```text
Reset
→ Moves branch/HEAD

Revert
→ Creates a new commit that undoes another commit
```

### What is `git stash`?

Temporarily stores uncommitted changes.

### What is cherry-pick?

Applies a specific commit from another branch.

### What is reflog?

Records movements of `HEAD` and branch references and can help recover lost commits.

### What is bisect?

A binary-search technique for finding the commit that introduced a bug.

### What is detached HEAD?

A state where `HEAD` points directly to a commit instead of a branch.

### What is `.gitignore`?

A file containing patterns for files Git should not track.
