\# Security Checks



This repository uses pre-commit hooks to perform security and repository

hygiene checks before a commit is created.



\## Secret detection



Gitleaks scans staged changes for potentially exposed secrets, including

API keys, access tokens, passwords, private keys, and other credentials.



If Gitleaks detects a potential secret, the commit is blocked.



\### If Gitleaks blocks your commit



1\. Review the file and line reported by Gitleaks.

2\. Remove the secret from the changes being committed.

3\. If the detected value is a real credential, revoke or rotate it immediately.

4\. Replace the credential with an environment variable or approved secret-management mechanism.

5\. Stage the corrected changes.

6\. Run the commit again.



Do not commit real credentials to the repository.



\### False positives



If you believe Gitleaks has detected a value that is not actually a secret,

do not simply bypass the security check.



Discuss the finding with the repository maintainers before adding an exception

or changing the Gitleaks configuration.



\## Other pre-commit checks



The repository also checks for:



\- Invalid YAML

\- Invalid JSON

\- Trailing whitespace

\- Missing end-of-file newlines

\- Unresolved Git merge conflicts

\- Accidentally added large files
