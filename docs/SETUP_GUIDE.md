# Quick Setup Guide

This guide will help you quickly set up this repository with proper governance rules.

## Initial Setup Checklist

### Step 1: Configure Teams (GitHub Organization)

Create the following teams in your GitHub organization:

```bash
# Team Structure
Opendashboard-Inc/
├── admins          # Repository administrators
├── maintainers     # Core maintainers
├── developers      # Regular contributors  
├── docs-team       # Documentation maintainers
├── security-team   # Security reviewers
└── devops-team     # DevOps and CI/CD maintainers
```

**How to create teams**:
1. Go to your organization page
2. Click "Teams" tab
3. Click "New team"
4. Name the team and set visibility
5. Add members to each team

### Step 2: Configure Repository Settings

#### General Settings

1. Navigate to **Settings** → **General**
2. Configure the following:

```
[ ] Allow merge commits
[x] Allow squash merging (recommended)
[ ] Allow rebase merging
[x] Automatically delete head branches
[x] Allow auto-merge
```

#### Branch Protection Rules

1. Navigate to **Settings** → **Branches**
2. Click **Add rule**
3. For `main` branch, configure:

```yaml
Branch name pattern: main

Protection rules:
  [x] Require a pull request before merging
      Required approvals: 2
      [x] Dismiss stale pull request approvals when new commits are pushed
      [x] Require review from Code Owners
      
  [x] Require status checks to pass before merging
      [x] Require branches to be up to date before merging
      Required checks:
        - validate-pr / Validate Pull Request
        - codeowners-check / Check CODEOWNERS
        - security-check / Security Checks
      
  [x] Require conversation resolution before merging
  [x] Require signed commits (optional but recommended)
  [x] Require linear history (optional)
  [x] Include administrators
  
  Restrictions:
    [ ] Restrict who can push to matching branches
        (Leave empty - all changes via PRs)
```

### Step 3: Enable Security Features

1. **Secret Scanning**:
   - Settings → Security → Code security and analysis
   - Enable "Secret scanning"
   - Enable "Push protection" for secret scanning

2. **Dependabot**:
   - Settings → Security → Code security and analysis
   - Enable "Dependabot alerts"
   - Enable "Dependabot security updates"

3. **Code Scanning** (recommended):
   - Settings → Security → Code security and analysis
   - Enable "Code scanning"
   - Set up CodeQL analysis

### Step 4: Update CODEOWNERS

Edit the `CODEOWNERS` file to match your team structure:

```bash
# Replace placeholder team names with actual teams
sed -i 's/@Opendashboard-Inc\/admins/@YourOrg\/admins/g' CODEOWNERS
sed -i 's/@Opendashboard-Inc\/docs-team/@YourOrg\/docs-team/g' CODEOWNERS
# Repeat for all teams
```

Or manually edit and replace:
- `@Opendashboard-Inc/admins` → `@YourOrg/admins`
- `@Opendashboard-Inc/docs-team` → `@YourOrg/docs-team`
- etc.

### Step 5: Configure Dependabot

The `.github/dependabot.yml` file is already configured for GitHub Actions. Uncomment and configure sections for your project's package managers:

- npm/yarn (JavaScript/TypeScript)
- pip (Python)
- maven/gradle (Java)
- go modules (Go)
- composer (PHP)
- etc.

### Step 6: Set Up Permissions

1. Navigate to **Settings** → **Collaborators and teams**
2. Add teams with appropriate permissions:

```
Team              | Permission Level
------------------|-----------------
admins            | Admin
maintainers       | Maintain
developers        | Write
docs-team         | Write
security-team     | Write
devops-team       | Write
```

### Step 7: Configure GitHub Actions

1. Navigate to **Settings** → **Actions** → **General**
2. Configure permissions:

```
Workflow permissions:
  ( ) Read and write permissions
  (x) Read repository contents and packages permissions

[x] Allow GitHub Actions to create and approve pull requests
```

### Step 8: Test Your Setup

#### Test Branch Protection

```bash
# Try to push directly to main (should fail)
git checkout main
git commit --allow-empty -m "test direct push"
git push origin main
# Expected: Push rejected due to branch protection

# Correct way: via PR
git checkout -b test-branch
git commit --allow-empty -m "test via PR"
git push origin test-branch
# Then create a PR via GitHub UI
```

#### Test CODEOWNERS

1. Create a PR that modifies a file owned by a specific team
2. Verify the correct team is automatically requested for review

#### Test Workflows

1. Create a test PR
2. Verify all workflow checks run:
   - PR Validation
   - CODEOWNERS Check
   - Security Check
3. Check that PR is labeled by size

## Quick Configuration Scripts

### Using GitHub CLI

```bash
# Install GitHub CLI
# https://cli.github.com/

# Enable branch protection
gh api repos/{owner}/{repo}/branches/main/protection \
  -X PUT \
  -f required_pull_request_reviews[required_approving_review_count]=2 \
  -f required_pull_request_reviews[require_code_owner_reviews]=true \
  -f required_pull_request_reviews[dismiss_stale_reviews]=true

# Enable security features
gh api repos/{owner}/{repo} \
  -X PATCH \
  -f security_and_analysis[secret_scanning][status]=enabled \
  -f security_and_analysis[secret_scanning_push_protection][status]=enabled
```

### Using Terraform

See `docs/REPOSITORY_RULES.md` for Terraform examples.

## Verification Checklist

After setup, verify:

- [ ] Branch protection rules are active on `main`
- [ ] CODEOWNERS file has correct team names
- [ ] All teams exist and have members
- [ ] Secret scanning is enabled
- [ ] Dependabot is configured
- [ ] Workflows run successfully on test PR
- [ ] Direct pushes to main are blocked
- [ ] PRs require specified number of approvals
- [ ] Code owners are auto-assigned on PRs
- [ ] Status checks block merging

## Troubleshooting

### Common Issues

**Issue**: "Required status checks not found"
- **Solution**: Create a PR first to trigger workflows, then the checks will appear in branch protection options

**Issue**: "CODEOWNERS file not working"
- **Solution**: 
  - Ensure file is in root directory or `.github/` directory
  - Verify team names are correct (including @org/)
  - Check file has no syntax errors

**Issue**: "Workflows not running"
- **Solution**:
  - Check Actions permissions in Settings → Actions
  - Verify workflow files have no syntax errors
  - Check branch/event triggers match your scenario

**Issue**: "Can't merge despite approvals"
- **Solution**:
  - Check all required status checks pass
  - Verify all conversations are resolved
  - Ensure branch is up to date with base branch

## Additional Configuration

### Add More Workflows

Based on your project type:

**For Node.js projects**:
```yaml
# .github/workflows/node-ci.yml
name: Node.js CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - run: npm ci
      - run: npm test
      - run: npm run build
```

**For Python projects**:
```yaml
# .github/workflows/python-ci.yml
name: Python CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
      - run: pip install -r requirements.txt
      - run: pytest
```

### Custom Labels

Add custom labels for better organization:

```bash
# Using GitHub CLI
gh label create "priority:high" --color "d73a4a"
gh label create "priority:medium" --color "fbca04"
gh label create "priority:low" --color "0e8a16"
gh label create "type:bug" --color "d73a4a"
gh label create "type:feature" --color "a2eeef"
gh label create "status:in-review" --color "fbca04"
```

## Next Steps

After completing this setup:

1. **Document Custom Processes**: Add any org-specific workflows to CONTRIBUTING.md
2. **Train Team**: Ensure all contributors understand the governance model
3. **Regular Audits**: Schedule quarterly reviews of permissions and rules
4. **Iterate**: Adjust rules based on team feedback and needs

## Support

If you need help:
- Review [docs/REPOSITORY_RULES.md](REPOSITORY_RULES.md) for detailed information
- Check GitHub's official documentation
- Open an issue in this repository

---

Remember: This is a template. Customize it to fit your organization's specific needs!
