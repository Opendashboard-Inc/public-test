# Security Policy

## Supported Versions

This section tells you which versions of the project are currently being supported with security updates.

| Version | Supported          |
| ------- | ------------------ |
| main    | :white_check_mark: |

## Reporting a Vulnerability

We take the security of this project seriously. If you discover a security vulnerability, please follow these steps:

### How to Report

**Please DO NOT report security vulnerabilities through public GitHub issues.**

Instead, please report them via one of the following methods:

1. **Email**: Send details to security@opendashboard-inc.example.com
2. **GitHub Security Advisory**: Use the [GitHub Security Advisory](https://github.com/Opendashboard-Inc/public-test/security/advisories/new) feature (preferred)
3. **Private Vulnerability Reporting**: Use GitHub's private vulnerability reporting feature if enabled

### What to Include

When reporting a vulnerability, please include:

- Type of vulnerability
- Full paths of source file(s) related to the vulnerability
- Location of the affected source code (tag/branch/commit or direct URL)
- Step-by-step instructions to reproduce the issue
- Proof-of-concept or exploit code (if possible)
- Impact of the vulnerability, including how an attacker might exploit it

### What to Expect

- **Acknowledgment**: We'll acknowledge receipt of your vulnerability report within 48 hours
- **Communication**: We'll send you regular updates about our progress
- **Timeline**: We aim to resolve critical issues within 30 days
- **Credit**: We'll credit you in the security advisory (unless you prefer to remain anonymous)

### Security Update Process

1. The security team will investigate and validate the report
2. We'll develop a fix and test it thoroughly
3. We'll prepare a security advisory
4. We'll release the security update
5. We'll publish the security advisory

## Security Best Practices

When contributing to this repository:

- Never commit secrets, API keys, passwords, or tokens
- Use environment variables for sensitive configuration
- Keep dependencies up to date
- Follow secure coding practices
- Review code for security issues before submitting PRs
- Use automated security scanning tools

## Disclosure Policy

- **Coordinated Disclosure**: We follow a coordinated disclosure policy
- **Public Disclosure**: Security vulnerabilities will be publicly disclosed after a fix is released
- **Embargo Period**: We request a 90-day embargo period for critical vulnerabilities

## Security Features

This repository implements the following security measures:

- Branch protection rules requiring code review
- Required status checks before merging
- Automated dependency scanning
- Code scanning for security vulnerabilities
- Secret scanning to prevent credential leaks

## Contact

For any security-related questions or concerns, contact:
- Security Team: security@opendashboard-inc.example.com
- Repository Maintainers: See CODEOWNERS file

## Bug Bounty Program

Currently, we do not have a bug bounty program. However, we greatly appreciate responsible disclosure and will acknowledge security researchers who report valid vulnerabilities.

---

Thank you for helping keep this project secure! 🔒
