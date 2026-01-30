# public-test

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Repository Health](https://github.com/Opendashboard-Inc/public-test/actions/workflows/repository-health.yml/badge.svg)](https://github.com/Opendashboard-Inc/public-test/actions/workflows/repository-health.yml)

## Overview

This repository serves as a **Proof of Concept (POC)** for testing repository governance, permissions, and rules. It's designed to help organizations validate their repository management strategy before open sourcing a project.

## Purpose

This POC demonstrates:

- ✅ **Branch Protection Rules**: Enforcing code review and quality standards
- ✅ **Code Ownership**: Using CODEOWNERS for automatic reviewer assignment
- ✅ **Automated Checks**: GitHub Actions workflows for validation and security
- ✅ **Contribution Guidelines**: Clear processes for contributors
- ✅ **Security Policies**: Vulnerability reporting and secret scanning
- ✅ **Issue/PR Templates**: Standardized formats for issues and pull requests
- ✅ **Dependency Management**: Automated updates via Dependabot

## Repository Structure

```
.
├── .github/
│   ├── ISSUE_TEMPLATE/        # Issue templates (bug, feature, question)
│   ├── workflows/             # GitHub Actions workflows
│   ├── dependabot.yml         # Dependabot configuration
│   └── PULL_REQUEST_TEMPLATE.md
├── docs/
│   └── REPOSITORY_RULES.md    # Comprehensive guide to repository governance
├── CODEOWNERS                 # Code ownership definitions
├── CONTRIBUTING.md            # Contribution guidelines
├── LICENSE                    # MIT License
├── README.md                  # This file
└── SECURITY.md               # Security policy
```

## Getting Started

### For Contributors

1. Read the [Contributing Guidelines](CONTRIBUTING.md)
2. Review the [Repository Rules](docs/REPOSITORY_RULES.md)
3. Check the [Security Policy](SECURITY.md)
4. Fork the repository and submit your changes via pull request

### For Administrators

1. **Review Governance Documents**:
   - [REPOSITORY_RULES.md](docs/REPOSITORY_RULES.md) - Complete guide to permissions and rules
   - [CODEOWNERS](CODEOWNERS) - Code ownership configuration
   - [SECURITY.md](SECURITY.md) - Security and vulnerability reporting

2. **Configure Branch Protection**:
   - Navigate to Settings → Branches
   - Add protection rules for `main` branch
   - See [docs/REPOSITORY_RULES.md](docs/REPOSITORY_RULES.md) for recommended settings

3. **Set Up Teams**:
   - Create teams: admins, maintainers, developers, docs-team, security-team, devops-team
   - Assign appropriate permissions to each team
   - Update CODEOWNERS file with actual team names

4. **Enable Security Features**:
   - Settings → Security → Enable secret scanning
   - Settings → Security → Enable Dependabot alerts
   - Settings → Security → Enable code scanning (CodeQL)

## Key Features

### 🛡️ Branch Protection

Protected branches require:
- Pull request reviews (configurable number)
- Status checks to pass
- Code owner approval
- Conversation resolution
- Up-to-date branches before merging

### 👥 Code Ownership

The `CODEOWNERS` file automatically assigns reviewers based on:
- File paths
- Directory patterns
- Specific file types
- Team ownership

### 🤖 Automated Workflows

**PR Validation** (`pr-validation.yml`):
- Validates PR title format
- Checks PR description completeness
- Scans for potential secrets
- Labels PRs by size
- Validates CODEOWNERS syntax

**Repository Health** (`repository-health.yml`):
- Checks for required governance files
- Validates repository structure
- Generates health reports
- Runs weekly and on-demand

### 📝 Templates

Standardized templates for:
- Bug reports
- Feature requests
- Questions
- Pull requests

### 🔒 Security

- Secret scanning for credentials
- Dependabot for dependency updates
- Security policy for vulnerability reporting
- Automated security checks in CI/CD

## Testing Repository Rules

This repository is designed to test various governance scenarios:

1. **Permission Testing**:
   - Test different user roles (read, write, maintain, admin)
   - Verify branch protection enforcement
   - Test CODEOWNERS reviewer assignment

2. **Workflow Testing**:
   - Ensure all automated checks run correctly
   - Verify status checks block merging when failing
   - Test PR labeling and validation

3. **Security Testing**:
   - Attempt to commit secrets (should be caught)
   - Test vulnerability scanning
   - Verify Dependabot functionality

## Common Scenarios

### Scenario 1: External Contributor

1. Fork the repository
2. Create a feature branch
3. Make changes and open a PR
4. Wait for automated checks
5. Address review feedback
6. Get approval from code owners
7. Changes are merged by maintainer

### Scenario 2: Emergency Hotfix

1. Create hotfix branch from main
2. Make critical fix with minimal changes
3. Request expedited review
4. Require admin approval
5. Merge with all checks passing
6. Create post-mortem issue

### Scenario 3: Major Feature

1. Create feature branch
2. Implement with tests and documentation
3. Open PR with detailed description
4. Undergo thorough review
5. Address all feedback
6. Get required approvals
7. Merge when ready

## Documentation

- **[CONTRIBUTING.md](CONTRIBUTING.md)**: How to contribute to this project
- **[SECURITY.md](SECURITY.md)**: Security policy and vulnerability reporting
- **[docs/REPOSITORY_RULES.md](docs/REPOSITORY_RULES.md)**: Comprehensive repository governance guide
- **[CODEOWNERS](CODEOWNERS)**: Code ownership definitions

## Workflows

| Workflow | Purpose | Trigger |
|----------|---------|---------|
| PR Validation | Validates pull requests | On PR open/update |
| Repository Health | Checks repo health | Weekly + Manual |

## Permissions Model

```
Admins (Full Control)
  └─ Maintainers (Manage without sensitive access)
      └─ Contributors (Write access)
          └─ External Contributors (Fork & PR)
```

## Adapting for Your Project

To use this POC as a template:

1. **Update Team Names**: Replace `@Opendashboard-Inc/*` with your org/teams in CODEOWNERS
2. **Customize Workflows**: Adjust workflow checks for your tech stack
3. **Add Language-Specific Tools**: Configure linters, test runners, etc.
4. **Update Documentation**: Modify docs to match your project needs
5. **Configure Dependabot**: Enable package ecosystems you use
6. **Add CI/CD**: Implement build and deployment workflows

## Resources

- [GitHub Docs: Managing Access](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings)
- [GitHub Docs: Branch Protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches)
- [GitHub Docs: CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Conventional Commits](https://www.conventionalcommits.org/)

## Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

## Security

For security concerns, please see our [Security Policy](SECURITY.md).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Note**: This is a POC repository. Adapt the governance model to fit your organization's needs before using in production.