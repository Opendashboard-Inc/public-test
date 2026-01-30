# Testing Repository Rules - Example Scenarios

This document provides example scenarios you can use to test the repository rules configured in this POC.

## Test Scenarios

### 1. Test Branch Protection (Direct Push Blocked)

**What to test**: Direct pushes to the `main` branch should be blocked.

**Steps**:
```bash
# This should fail
git checkout main
git commit --allow-empty -m "test: direct push to main"
git push origin main
# Expected: "remote: error: GH006: Protected branch update failed"
```

**Expected Result**: Push is rejected due to branch protection rules.

### 2. Test Pull Request Flow

**What to test**: Changes must go through pull requests.

**Steps**:
```bash
# Create a feature branch
git checkout -b feature/test-governance

# Make a change
echo "# Test" > test-file.md
git add test-file.md
git commit -m "feat: add test file"
git push origin feature/test-governance

# Create PR via GitHub UI or CLI
gh pr create --title "feat: test governance rules" --body "Testing PR workflow"
```

**Expected Result**: 
- PR is created successfully
- Automated checks run (PR Validation workflow)
- PR requires approvals before merge
- All status checks must pass

### 3. Test CODEOWNERS

**What to test**: Code owners are automatically requested for review.

**Steps**:
```bash
# Create branch
git checkout -b test/codeowners

# Modify a file owned by specific team
echo "# Updated docs" >> README.md
git add README.md
git commit -m "docs: update README"
git push origin test/codeowners

# Create PR
gh pr create --title "docs: test CODEOWNERS" --body "Testing code ownership"
```

**Expected Result**:
- PR is created
- Teams/users from CODEOWNERS are automatically requested as reviewers
- `@Opendashboard-Inc/docs-team` should be requested (or your configured teams)

### 4. Test PR Template

**What to test**: PR template is used when creating pull requests.

**Steps**:
1. Create a new branch and make changes
2. Go to GitHub and create a new PR
3. Observe that the PR description is pre-filled with the template

**Expected Result**: PR description includes the checklist and sections from the template.

### 5. Test Issue Templates

**What to test**: Issue templates guide users to provide necessary information.

**Steps**:
1. Go to Issues → New Issue
2. Select "Bug Report" template
3. Fill out the template

**Expected Result**: Issue is created with structured information.

### 6. Test Workflow - PR Validation

**What to test**: Automated PR validation checks.

**Steps**:
```bash
git checkout -b test/pr-validation
git commit --allow-empty -m "test: PR validation"
git push origin test/pr-validation
gh pr create --title "test: check validation" --body ""
```

**Expected Result**:
- Workflow runs automatically
- Check fails due to empty PR description
- Warning about missing conventional commit format if not followed

### 7. Test Workflow - Repository Health

**What to test**: Repository health check workflow.

**Steps**:
```bash
# Manually trigger the workflow
gh workflow run repository-health.yml

# Check the run
gh run list --workflow=repository-health.yml
```

**Expected Result**:
- Workflow runs successfully
- Checks for required files
- Generates health report
- Artifact is created with the report

### 8. Test Secret Detection

**What to test**: Secrets should be caught by PR validation.

**Steps**:
```bash
git checkout -b test/secret-detection
echo 'password="my_secure_password_12345"' > config.js
git add config.js
git commit -m "test: potential secret"
git push origin test/secret-detection
gh pr create --title "test: secret scanning" --body "Testing secret detection"
```

**Expected Result**:
- PR is created
- Security check workflow fails
- Warning/error about potential secret in code

### 9. Test PR Size Labeling

**What to test**: PRs are automatically labeled by size.

**Steps**:
Create PRs with different sizes:

**Small PR**:
```bash
git checkout -b test/small-pr
echo "small change" > small.txt
git add small.txt && git commit -m "feat: small change"
git push origin test/small-pr
gh pr create --title "feat: small PR" --body "Testing size labeling"
```

**Large PR**:
```bash
git checkout -b test/large-pr
for i in {1..100}; do echo "line $i" >> large.txt; done
git add large.txt && git commit -m "feat: large change"
git push origin test/large-pr
gh pr create --title "feat: large PR" --body "Testing size labeling"
```

**Expected Result**: PRs receive size labels (size/XS, size/S, size/M, size/L, size/XL).

### 10. Test Dependabot

**What to test**: Dependabot creates PRs for dependency updates.

**Prerequisites**: Add a package file (package.json, requirements.txt, etc.)

**Steps**:
1. Wait for Dependabot to scan (or manually trigger)
2. Check for Dependabot PRs in the repository

**Expected Result**:
- Dependabot creates PRs for outdated dependencies
- PRs include changelog and compatibility information

## Testing Permissions

### For Administrators

Test that admins can:
- [x] Access repository settings
- [x] Modify branch protection rules
- [x] Manage teams and permissions
- [x] Force merge (if not restricted)
- [x] Delete branches

### For Maintainers

Test that maintainers can:
- [x] Approve PRs
- [x] Merge approved PRs
- [x] Manage issues
- [x] Create releases
- [ ] Cannot modify repository settings
- [ ] Cannot change branch protection

### For Contributors

Test that contributors can:
- [x] Create branches
- [x] Push to their branches
- [x] Create PRs
- [x] Comment on issues/PRs
- [ ] Cannot merge PRs
- [ ] Cannot push to main
- [ ] Cannot modify workflows

### For External Contributors

Test that external contributors can:
- [x] Fork the repository
- [x] Create PRs from forks
- [x] Comment on their PRs
- [ ] Cannot push to repository branches
- [ ] Cannot merge PRs
- [ ] Cannot close others' issues

## Verification Checklist

After running these tests, verify:

- [ ] Direct pushes to `main` are blocked
- [ ] All changes require pull requests
- [ ] PRs require minimum number of approvals
- [ ] Code owners are auto-requested on PRs
- [ ] Status checks must pass before merge
- [ ] PR and issue templates work correctly
- [ ] Workflows run automatically
- [ ] Secret scanning catches potential leaks
- [ ] Dependabot is configured and working
- [ ] Permissions are correctly enforced

## Troubleshooting Common Issues

### Workflow Not Running

**Problem**: Workflow doesn't trigger on PR
**Solution**: 
- Check workflow file syntax
- Verify trigger conditions match (branches, events)
- Check Actions permissions in repository settings

### CODEOWNERS Not Working

**Problem**: Code owners not auto-requested
**Solution**:
- Verify CODEOWNERS file location (root or `.github/`)
- Check team names are correct (include `@org/`)
- Ensure requester has correct permissions

### Can't Merge PR

**Problem**: Merge button disabled despite approvals
**Solution**:
- Check all required status checks pass
- Verify branch is up to date
- Ensure all conversations resolved
- Check you have merge permissions

## Advanced Testing

### Test Branch Protection Bypass

**For Admins Only**: Test if admin override works (if enabled)

```bash
# If "Include administrators" is unchecked, admins can bypass
git push origin main  # Should work for admins only
```

### Test Signed Commits

If signed commits are required:

```bash
# Configure GPG
git config --global user.signingkey YOUR_KEY_ID
git config --global commit.gpgsign true

# Make signed commit
git commit -S -m "feat: signed commit"
```

### Test Linear History

If linear history is required:

```bash
# This should be blocked
git merge --no-ff feature-branch

# This should work
git rebase main
```

## Cleanup

After testing, clean up test branches:

```bash
# Delete local branches
git branch -D test/secret-detection test/codeowners feature/test-governance

# Delete remote branches
git push origin --delete test/secret-detection test/codeowners feature/test-governance
```

---

**Note**: Replace `@Opendashboard-Inc/` with your organization name when testing CODEOWNERS functionality.
