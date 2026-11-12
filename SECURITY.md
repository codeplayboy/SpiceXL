# Security and privacy

SpiceXL is a client application. The public repository contains presentation assets, release artifacts, and documentation only. It must never contain provider credentials, API keys, session cookies, access tokens, private signing material, or unpublished infrastructure details.

## Repository guarantees

- No `.env` files, signing keys, certificates, cookies, credentials, or access tokens are part of the published tree.
- The Android APK and Windows installer are distributables, not source containers for backend secrets.
- The client communicates with a public HTTPS gateway. Secrets belong in the server-side environment, never in JavaScript, an APK, an installer, or screenshots.
- Provider names, route diagnostics, and language labels are runtime data. They are not credentials and are not hardcoded into this repository.

## Safe contribution rules

Before opening a pull request, inspect staged files and confirm that no local configuration or generated secret material is included:

```bash
git diff --cached --name-only
git grep -n -I -E "(api[_-]?key|client[_-]?secret|password|authorization:|bearer |private[_-]?key|access[_-]?token|refresh[_-]?token)" -- .
```

The repository `.gitignore` blocks common local secret formats. It is still the contributor's responsibility to review the staged diff.

## Reporting a vulnerability

Do not publish credentials or exploit details in an issue. Revoke exposed credentials immediately, then contact the project maintainer privately with a minimal reproduction and the affected release or commit.
