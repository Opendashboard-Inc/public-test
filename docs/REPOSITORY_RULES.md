# Repository Rules and Permissions Guide

This document outlines the repository governance model, permissions structure, and best practices for managing access and rules in this repository.

## Table of Contents

- [Overview](#overview)
- [Repository Permissions](#repository-permissions)
- [Branch Protection Rules](#branch-protection-rules)
- [Code Review Requirements](#code-review-requirements)
- [Automated Checks](#automated-checks)
- [Security Policies](#security-policies)
- [Best Practices](#best-practices)

## Overview

This repository uses a comprehensive governance model designed to:
- Ensure code quality through mandatory reviews
- Protect critical branches from unauthorized changes
- Automate security and compliance checks
- Define clear ownership of code areas
- Maintain transparency in contributions

## Repository Permissions

### Permission Levels

GitHub provides several permission levels:

1. **Read**: Can view and clone the repository
2. **Triage**: Can manage issues and pull requests without write access
3. **Write**: Can push to the repository and manage issues/PRs
4. **Maintain**: Can manage the repository without access to sensitive settings
5. **Admin**: Full access including settings and security

### Recommended Structure

```
┌─────────────────────────────────────────────────────┐
│                    Admins                           │
│  - Repository settings                              │
│  - Security & permissions                           │
│  - Merge to protected branches                      │
└─────────────────────────────────────────────────────┘
                       │
┌─────────────────────────────────────────────────────┐
│                 Maintainers                         │
│  - Manage releases                                  │
│  - Triage issues                                    │
│  - Review & approve PRs                             │
└─────────────────────────────────────────────────────┘
                       │
┌─────────────────────────────────────────────────────┐
│                  Contributors                       │
│  - Create branches                                  │
│  - Submit pull requests                             │
│  - Participate in reviews                           │
└─────────────────────────────────────────────────────┘
                       │
┌─────────────────────────────────────────────────────┐
│               External Contributors                 │
│  - Fork repository                                  │
│  - Submit pull requests from forks                  │
└─────────────────────────────────────────────────────┘
```

### Teams

Configure teams for easier permission management:

- **@Opendashboard-Inc/admins**: Repository administrators
- **@Opendashboard-Inc/maintainers**: Core maintainers
- **@Opendashboard-Inc/developers**: Regular contributors
- **@Opendashboard-Inc/docs-team**: Documentation maintainers
- **@Opendashboard-Inc/security-team**: Security reviewers
- **@Opendashboard-Inc/devops-team**: DevOps and CI/CD maintainers

## Branch Protection Rules

### Main Branch Protection

**Recommended settings for `main` branch:**

#### Required Reviews
- [x] Require pull request reviews before merging
  - Required approving reviews: **2**
  - Dismiss stale pull request approvals when new commits are pushed
  - Require review from Code Owners
  - Restrict who can dismiss pull request reviews: **Admins only**

#### Status Checks
- [x] Require status checks to pass before merging
  - [x] Require branches to be up to date before merging
  - Required checks:
    - PR Validation
    - Security Checks
    - Tests (when applicable)
    - Linting (when applicable)

#### Additional Restrictions
- [x] Require conversation resolution before merging
- [x] Require signed commits
- [x] Require linear history
- [x] Include administrators (enforce rules on admins too)
- [ ] Allow force pushes (should be disabled)
- [ ] Allow deletions (should be disabled)

#### Who can push to matching branches
- Restrict to: **None** (all changes must go through PRs)

### Development Branch Protection

**Recommended settings for `develop` or `staging` branch:**

- [x] Require pull request reviews before merging
  - Required approving reviews: **1**
- [x] Require status checks to pass before merging
- [x] Require conversation resolution before merging
- [ ] Require linear history (optional for dev branches)

### Release Branch Protection

**Recommended settings for `release/*` branches:**

- [x] Require pull request reviews before merging
  - Required approving reviews: **2**
  - Require review from Code Owners
- [x] Require status checks to pass before merging
- [x] Require signed commits
- [x] Include administrators

## Code Review Requirements

### CODEOWNERS File

The `CODEOWNERS` file defines who must review changes to specific parts of the codebase:

```
# Example CODEOWNERS
* @Opendashboard-Inc/admins
/docs/ @Opendashboard-Inc/docs-team
/src/ @Opendashboard-Inc/developers
/.github/ @Opendashboard-Inc/devops-team
SECURITY.md @Opendashboard-Inc/security-team
```

### Review Best Practices

**For Reviewers:**
1. Review code for correctness, not just style
2. Look for security vulnerabilities
3. Ensure tests are adequate
4. Check documentation is updated
5. Verify breaking changes are clearly documented
6. Be respectful and constructive

**For Authors:**
1. Keep PRs small and focused
2. Write clear descriptions
3. Respond to all review comments
4. Update your PR based on feedback
5. Ensure all checks pass before requesting review

## Automated Checks

### Required Workflows

1. **PR Validation** (`pr-validation.yml`)
   - Validates PR title format
   - Checks for linked issues
   - Scans for secrets
   - Validates CODEOWNERS
   - Labels PR by size

2. **Repository Health** (`repository-health.yml`)
   - Checks for required files
   - Validates governance documents
   - Generates health reports

### Recommended Additional Checks

Depending on your project type, consider adding:

- **Code Quality**: Linters, formatters, static analysis
- **Security**: CodeQL, dependency scanning, secret scanning
- **Testing**: Unit tests, integration tests, coverage reports
- **Build**: Ensure project builds successfully
- **Documentation**: Verify docs build and links work

## Security Policies

### Secret Scanning

GitHub automatically scans for known secret patterns:
- Enable for private repositories: Settings → Security → Secret scanning
- Configure custom patterns for your specific needs
- Set up automatic remediation workflows

### Dependency Scanning

Dependabot is configured to:
- Check for vulnerable dependencies weekly
- Open PRs for security updates immediately
- Group related updates

### Code Scanning

Configure CodeQL for automated security analysis:
```yaml
# .github/workflows/codeql.yml
name: "CodeQL"
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 0 * * 0'
```

### Vulnerability Reporting

See `SECURITY.md` for:
- How to report security vulnerabilities
- Expected response times
- Disclosure policy

## Best Practices

### For Repository Administrators

1. **Regular Audits**
   - Review access permissions quarterly
   - Audit repository settings
   - Check for inactive users
   - Verify team memberships

2. **Documentation**
   - Keep governance docs up to date
   - Document any custom rules or processes
   - Maintain changelog for policy changes

3. **Automation**
   - Use GitHub Actions for repetitive tasks
   - Implement automated security scanning
   - Set up notifications for security alerts

4. **Training**
   - Onboard new contributors properly
   - Share security best practices
   - Document common workflows

### For Contributors

1. **Before Contributing**
   - Read `CONTRIBUTING.md`
   - Review existing issues and PRs
   - Discuss major changes first

2. **Submitting Changes**
   - Fork the repository (external contributors)
   - Create feature branches
   - Write clear commit messages
   - Ensure tests pass locally

3. **During Review**
   - Be responsive to feedback
   - Keep PRs updated
   - Resolve conversations
   - Be patient and respectful

### Common Patterns

#### Emergency Hotfixes

For critical production issues:
1. Create hotfix branch from `main`
2. Make minimal necessary changes
3. Get expedited review (still required)
4. Merge with admin approval
5. Create post-mortem issue

#### Release Process

1. Create release branch from `main`
2. Bump version numbers
3. Update changelog
4. Tag release
5. Merge to `main` with required reviews
6. Deploy from tag

#### Feature Development

1. Create feature branch from `main`
2. Implement feature with tests
3. Open PR when ready for review
4. Address review feedback
5. Merge when approved and checks pass

## Configuring These Rules in GitHub

### Via GitHub UI

1. Go to repository **Settings**
2. Navigate to **Branches**
3. Click **Add rule** under "Branch protection rules"
4. Configure settings as documented above
5. Click **Create** or **Save changes**

### Via GitHub API

Use the [Branch Protection API](https://docs.github.com/en/rest/branches/branch-protection):

```bash
# Example: Protect main branch
curl -X PUT \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: token YOUR_TOKEN" \
  https://api.github.com/repos/OWNER/REPO/branches/main/protection \
  -d '{"required_pull_request_reviews":{"required_approving_review_count":2}}'
```

### Via Terraform

```hcl
resource "github_branch_protection" "main" {
  repository_id = github_repository.repo.node_id
  pattern       = "main"
  
  required_pull_request_reviews {
    required_approving_review_count = 2
    require_code_owner_reviews      = true
    dismiss_stale_reviews          = true
  }
  
  required_status_checks {
    strict = true
    contexts = ["PR Validation", "Tests"]
  }
}
```

## Testing Repository Rules

### Manual Testing

1. **Test Branch Protection**:
   - Try to push directly to protected branch (should fail)
   - Create PR without required approvals (should block merge)
   - Try to merge with failing checks (should block merge)

2. **Test CODEOWNERS**:
   - Create PR modifying files owned by a team
   - Verify correct reviewers are auto-assigned

3. **Test Workflows**:
   - Open PR with various formats
   - Verify all checks run correctly
   - Check that labels are applied

### Automated Testing

Create a test plan to verify:
- All protection rules work as expected
- Workflows execute successfully
- Permissions are correctly enforced
- CODEOWNERS file is valid

## Monitoring and Compliance

### Regular Checks

- Review security alerts weekly
- Check Dependabot PRs daily
- Audit access permissions monthly
- Review workflow run history

### Metrics to Track

- Number of security vulnerabilities
- Time to resolve security issues
- PR review turnaround time
- Percentage of PRs requiring fixes after review
- Number of direct pushes to protected branches (should be 0)

## Additional Resources

- [GitHub Docs: Managing Access](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings)
- [GitHub Docs: Branch Protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches)
- [GitHub Docs: CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [GitHub Docs: Security Features](https://docs.github.com/en/code-security)

---

**Note**: This is a living document. Update it as repository governance evolves.
