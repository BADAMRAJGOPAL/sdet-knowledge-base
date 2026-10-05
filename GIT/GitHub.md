# GitHub — SDET Cheatsheet

> GitHub concepts specifically relevant to SDET / QA Automation engineers.

---

# 1. What is GitHub?

GitHub is a platform used to:

* Host Git repositories
* Collaborate on code
* Review Pull Requests
* Manage issues
* Run CI/CD using GitHub Actions
* Store automation projects
* Manage releases and permissions

```text
Git
↓
Version Control

GitHub
↓
Hosting + Collaboration + CI/CD
```

---

# 2. GitHub Repository

A GitHub repository is a remote project repository.

Typical SDET repository:

```text
sdet-automation/
│
├── src/
├── tests/
├── test-data/
├── reports/
├── pom.xml
├── README.md
├── .gitignore
└── .github/
    └── workflows/
```

It can contain:

* Automation framework
* Test cases
* Test data
* Documentation
* CI/CD workflows
* Configuration

---

# 3. Repository Visibility

A GitHub repository can commonly be:

### Public

Anyone can view it.

### Private

Only authorized users can access it.

For company automation projects, repositories are usually private.

---

# 4. Local vs Remote Repository

```text
Your Computer
─────────────
Local Repository
       │
       │ git push
       ↓
GitHub
─────────────
Remote Repository
```

### Local

Your working copy and local Git history.

### Remote

Repository hosted on GitHub.

---

# 5. Fork

A fork creates a copy of another GitHub repository under your GitHub account.

```text
Original Repository
        ↓
       Fork
        ↓
Your GitHub Repository
```

Commonly used for open-source contribution.

---

# 6. Fork vs Clone

| Fork                                  | Clone                    |
| ------------------------------------- | ------------------------ |
| GitHub operation                      | Git operation            |
| Creates repository under your account | Creates local copy       |
| Happens on GitHub                     | Happens on your computer |
| Useful for contribution               | Useful for development   |

Usually:

```text
Fork
 ↓
Clone
 ↓
Modify
 ↓
Commit
 ↓
Push
 ↓
Pull Request
```

---

# 7. Origin and Upstream

In a fork workflow:

```text
origin
↓
Your fork

upstream
↓
Original repository
```

Add upstream:

```bash
git remote add upstream <original-repository-url>
```

Check:

```bash
git remote -v
```

Fetch original repository:

```bash
git fetch upstream
```

Update local main:

```bash
git switch main
git merge upstream/main
```

---

# 8. Pull Request

A Pull Request (PR) is a request to merge changes from one branch into another.

Example:

```text
feature-login
      ↓
Pull Request
      ↓
main
```

Typical SDET workflow:

```text
Create branch
     ↓
Write automation
     ↓
Commit
     ↓
Push
     ↓
Create PR
     ↓
Code Review
     ↓
CI
     ↓
Approval
     ↓
Merge
```

---

# 9. Draft Pull Request

A Draft PR indicates that work is still in progress.

Use it when:

* Implementation is incomplete
* You want early feedback
* You want CI to run
* You want reviewers to see the current work

Once ready, mark it as **Ready for review**.

---

# 10. Pull Request Code Review

Reviewers commonly check:

### Code

* Readability
* Naming
* Maintainability
* Duplicate code
* Design

### Automation

* Test reliability
* Assertions
* Test data
* Wait strategies
* Page Object usage
* API validation
* Error handling

### Git

* Meaningful commits
* Unnecessary files
* Secrets
* Generated files

---

# 11. PR Approval

A reviewer can approve a PR after reviewing the changes.

A repository can also have branch protection rules requiring:

* PR review
* Required approvals
* Passing CI checks
* No unresolved conversations

---

# 12. PR Merge Strategies

## Merge Commit

Preserves the branch history and creates a merge commit.

```text
A ── B ── C
     \    \
      D ── M
```

## Squash and Merge

Combines all PR commits into one commit.

```text
A ── B ── S
```

Useful for keeping the main branch history clean.

## Rebase and Merge

Creates a linear history by replaying commits.

```text
A ── B ── C ── D'
```

---

# 13. GitHub Issues

Issues are used to track work such as:

* Bugs
* Enhancements
* Tasks
* Automation improvements

Example:

```text
Issue #124
"Login automation fails when OTP expires"
```

For SDET teams, issues can track automation bugs and framework improvements.

---

# 14. Labels

Labels categorize issues and PRs.

Examples:

```text
bug
automation
enhancement
priority-high
blocked
```

Example:

```text
Bug #125
Labels:
bug
automation
priority-high
```

---

# 15. README.md

`README.md` is usually the first documentation file users see.

An SDET automation README can contain:

```text
# Automation Framework

## Tech Stack

- Java
- Selenium
- TestNG
- REST Assured
- Maven

## Setup

## How to Run Tests

## Test Reports

## Project Structure

## CI/CD
```

---

# 16. GitHub Actions

GitHub Actions provides CI/CD automation.

Example SDET pipeline:

```text
Developer Push
      ↓
GitHub Actions
      ↓
Checkout Code
      ↓
Setup Java
      ↓
Install Dependencies
      ↓
Run Tests
      ↓
Generate Report
      ↓
Publish Result
```

Workflow files are stored under:

```text
.github/
└── workflows/
    └── automation-tests.yml
```

---

# 17. GitHub Actions Basic Concepts

A workflow commonly contains:

```text
Workflow
   ↓
Event
   ↓
Job
   ↓
Steps
```

### Workflow

Complete automation definition.

### Event

What triggers it.

Examples:

```text
push
pull_request
schedule
workflow_dispatch
```

### Job

A group of tasks.

### Step

Individual command/action.

---

# 18. SDET GitHub Actions Example

A simplified workflow:

```yaml
name: Automation Tests

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          java-version: '17'

      - name: Run tests
        run: mvn test
```

The important concept:

```text
Code Change
    ↓
GitHub
    ↓
GitHub Actions
    ↓
Automation Tests
    ↓
Pass / Fail
```

---

# 19. GitHub Secrets

Secrets store sensitive information securely for workflows.

Examples:

```text
API_TOKEN
DB_PASSWORD
USERNAME
AWS credentials
```

In GitHub Actions:

```yaml
${{ secrets.API_TOKEN }}
```

### Never commit

```text
password
API key
access token
private key
```

into source code.

---

# 20. SSH Authentication

SSH allows Git operations without repeatedly entering credentials.

Typical repository URL:

```text
git@github.com:user/project.git
```

SSH generally uses:

```text
Private Key
     +
Public Key
     ↓
GitHub
```

### Important

Never share your private key.

---

# 21. HTTPS + PAT

GitHub HTTPS operations can use a Personal Access Token (PAT) instead of a password.

Example:

```text
https://github.com/user/project.git
```

### PAT security

Never:

* Commit it
* Share it
* Put it in source code
* Print it in CI logs

If compromised, revoke/rotate it.

---

# 22. Branch Protection

Branch protection helps prevent unsafe changes to important branches.

For example, `main` may require:

```text
Pull Request
     ↓
Code Review
     ↓
CI Checks Pass
     ↓
Approval
     ↓
Merge
```

This is especially useful for automation repositories used by teams.

---

# 23. GitHub Releases

A release can represent a stable version of a project.

Example:

```text
v1.0.0
v1.1.0
v2.0.0
```

For an SDET framework, a release might represent:

```text
Automation Framework v2.0
```

Releases are less important for day-to-day SDET work than Pull Requests and Actions.

---

# 24. GitHub SDET Workflow

```text
GitHub Repository
       ↓
Clone
       ↓
Create feature branch
       ↓
Develop automation
       ↓
Commit
       ↓
Push
       ↓
Pull Request
       ↓
Code Review
       ↓
GitHub Actions
       ↓
Automation Tests
       ↓
Approval
       ↓
Merge
       ↓
main
```

---

# 25. SDET Interview Questions

### What is GitHub?

A platform for hosting Git repositories and providing collaboration and CI/CD features.

### What is a Pull Request?

A request to merge changes from one branch into another.

### What is a Draft PR?

A PR that is still being developed and is not yet ready for final review/merge.

### What is a Fork?

A GitHub copy of a repository under another GitHub account.

### Fork vs Clone?

```text
Fork
→ GitHub copy

Clone
→ Local copy
```

### What is GitHub Actions?

GitHub's CI/CD automation platform.

### How would you run Selenium tests automatically?

```text
Push / PR
   ↓
GitHub Actions
   ↓
Setup environment
   ↓
Build
   ↓
Run Selenium tests
   ↓
Generate report
```

### What are GitHub Secrets?

Encrypted values used to securely provide sensitive information to workflows.

### Why use branch protection?

To prevent uncontrolled direct changes to important branches.

### What is the difference between Merge, Squash and Rebase?

```text
Merge
→ Preserves branch history

Squash
→ Combines PR commits into one

Rebase
→ Creates a linear history
```

---

# 26. One-Minute GitHub Revision

```text
Repository
↓
Remote project

Fork
↓
GitHub copy

Clone
↓
Local copy

Branch
↓
Feature development

Pull Request
↓
Request to merge

Code Review
↓
Review changes

Approval
↓
Reviewer accepts

GitHub Actions
↓
CI/CD

Secrets
↓
Secure credentials

Branch Protection
↓
Protect important branches
```

---

# 27. What an SDET Should Know

### Must Know

```text
Repository
Fork
Clone
Branch
Pull Request
Code Review
PR approval
Merge strategies
GitHub Actions
GitHub Secrets
Branch protection
SSH / PAT
```

### Practical SDET Flow

```text
Create automation
       ↓
Git branch
       ↓
Commit
       ↓
Push
       ↓
Pull Request
       ↓
Review
       ↓
CI
       ↓
Merge
```
