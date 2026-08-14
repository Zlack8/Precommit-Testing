# Gitleaks Pre-commit Test

## Precommit Testing

A proof-of-concept repository for testing **pre-commit security and repository hygiene checks** before proposing their use in the Mozilla SLIIT repositories.

## Purpose

This repository tests a standardized pre-commit configuration that runs checks before a Git commit is created.

The main security requirement is to detect accidentally committed secrets and prevent the commit from being created when a potential secret is found.

## Tools

* **Gitleaks** — detects potential secrets such as API keys, access tokens, passwords, and private keys.
* **pre-commit-hooks** — provides general repository hygiene checks.

### Current checks

* Gitleaks secret detection
* YAML validation
* JSON validation
* End-of-file fixing
* Trailing whitespace
* Merge conflict detection
* Large-file detection

## How It Works

The normal workflow is:

```text
git add
   │
   ▼
git commit
   │
   ▼
pre-commit
   │
   ├── Gitleaks
   ├── YAML/JSON checks
   └── Repository hygiene checks
   │
   ▼
All checks pass?
   │
   ├── No → Commit blocked
   │
   └── Yes → Commit created
```

Gitleaks checks the staged content that is being committed.

## Setup

### Requirements

* Git
* Python
* pre-commit

### Install pre-commit

If pre-commit is not already installed:

```powershell
pip install pre-commit
```

Install the pre-commit hooks after cloning the repository:

```powershell
pre-commit install
```
> Note: This assumes the developer has internet access.
Run all hooks manually:

```powershell
pre-commit run --all-files
```

After installation, the hooks run automatically during normal commits:

```powershell
git add .
git commit -m "Your commit message"
```

```text
Git
  ↓
Python
  ↓
pre-commit
  ↓
pre-commit install
  ↓
pre-commit reads .pre-commit-config.yaml
  ↓
Gitleaks hook is prepared
  ↓
git commit
  ↓
Gitleaks runs
```

## Security Test Results

The configuration has been tested against several scenarios.

| Test                                  | Expected result                        | Result    |
| ------------------------------------- | -------------------------------------- | --------- |
| Fake secret in a new file             | Commit blocked                         | Passed    |
| Fake secret added to an existing file | Commit blocked                         | Passed    |
| Normal change                         | Commit allowed                         | Passed    |
| Secret only in unstaged changes       | Not included in commit                 | Passed    |
| Secret already present in Git history | Not detected by normal pre-commit scan | Confirmed |

The historical-secret test was performed deliberately using a fake credential in this test repository.

## Important Limitation

The pre-commit configuration is intended to prevent **new secrets from being introduced through normal commits**.

It should not be considered a complete repository security solution.

In particular, the normal pre-commit Gitleaks hook does not perform a complete historical scan of the repository for every commit.

Additional controls may be appropriate for the final repository setup, such as CI/CD security scanning, dependency scanning, SAST, or historical secret scanning.

## Security Guidance

If Gitleaks detects a potential secret:

1. Do not bypass the check immediately.
2. Review the reported file and finding.
3. Remove the secret from the changes being committed.
4. If the value is a real credential, revoke or rotate it immediately.
5. Replace the credential with an environment variable or approved secret-management mechanism.
6. Stage the corrected changes and retry the commit.

The repository's `SECURITY.md` contains additional guidance.

## Status

This repository is currently a **proof of concept** for evaluating a standardized pre-commit configuration before applying an appropriate configuration to the Mozilla SLIIT repositories.


Safe test change
github_token=ghp_123456789012345678901234567890123456
This is an unrelated change.
