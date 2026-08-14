# Security

This repository uses pre-commit hooks to perform security and repository hygiene checks before commits are created.

## Secret Detection

Gitleaks scans staged changes for potential secrets, including:

* API keys
* Access tokens
* Passwords
* Private keys
* Other credentials

If Gitleaks detects a potential secret, the commit is blocked.

## If a Secret Is Detected

If your commit is blocked by Gitleaks:

1. Review the file and finding reported by Gitleaks.
2. Remove the secret from the changes being committed.
3. If the detected value is a real credential, revoke or rotate it immediately.
4. Replace the credential with an environment variable or an approved secret-management mechanism.
5. Stage the corrected changes.
6. Run the commit again.

Do not commit real credentials to the repository.

## False Positives

Gitleaks findings should be reviewed before assuming they are false positives.

If you believe a finding is not a real secret, discuss it with the repository maintainers before adding an exception or changing the Gitleaks configuration.

Exceptions should be used carefully and should not be used to bypass legitimate security findings.

## Scope

The pre-commit configuration is a local security control designed to prevent potential secrets from being introduced through normal commits.

It does not replace other security controls such as:

* CI/CD security scanning
* Dependency vulnerability scanning
* Static application security testing (SAST)
* Historical repository scanning
* Remote repository security controls

## Reporting a Real Credential Exposure

If a real credential has been committed or exposed:

1. Treat the credential as compromised.
2. Revoke or rotate it immediately.
3. Notify the appropriate repository/project maintainer.
4. Determine whether the credential exists in Git history or other locations.
5. Follow the organization's incident-response process.

Do not rely on simply deleting the credential from the latest version of a file. A credential may remain present in Git history.
