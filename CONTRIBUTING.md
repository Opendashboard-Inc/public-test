# Contributing to public-test

Thank you for your interest in contributing to this project! This document provides guidelines for contributing to this repository.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Commit Message Guidelines](#commit-message-guidelines)

## Code of Conduct

By participating in this project, you agree to abide by our code of conduct. Please be respectful and constructive in all interactions.

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/YOUR_USERNAME/public-test.git`
3. Create a new branch: `git checkout -b feature/your-feature-name`
4. Make your changes
5. Test your changes thoroughly
6. Commit your changes (see [Commit Message Guidelines](#commit-message-guidelines))
7. Push to your fork: `git push origin feature/your-feature-name`
8. Open a Pull Request

## How to Contribute

### Reporting Bugs

- Use the bug report issue template
- Include detailed steps to reproduce the issue
- Provide information about your environment
- Include screenshots if applicable

### Suggesting Enhancements

- Use the feature request issue template
- Clearly describe the proposed feature
- Explain why this enhancement would be useful
- Provide examples of how it would work

### Code Contributions

- Check existing issues and pull requests to avoid duplicates
- Discuss major changes in an issue before starting work
- Follow the coding standards outlined below
- Write clear, commented code
- Add tests for new functionality
- Update documentation as needed

## Pull Request Process

1. **Before Submitting**:
   - Ensure your code follows the project's coding standards
   - Run all tests and ensure they pass
   - Update documentation if needed
   - Rebase your branch on the latest main branch

2. **PR Requirements**:
   - Fill out the pull request template completely
   - Reference any related issues
   - Provide a clear description of the changes
   - Include screenshots for UI changes
   - Ensure all CI checks pass

3. **Review Process**:
   - Code owners will be automatically assigned as reviewers
   - Address all review comments
   - Maintain a respectful dialogue during review
   - Keep your PR updated with the main branch

4. **Merging**:
   - PRs require approval from code owners
   - All CI checks must pass
   - Merge conflicts must be resolved
   - Squash commits if requested

## Coding Standards

- Follow consistent code style with the existing codebase
- Write self-documenting code with clear variable and function names
- Add comments for complex logic
- Keep functions small and focused
- Write unit tests for new functionality
- Ensure code is properly formatted

## Commit Message Guidelines

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that don't affect code meaning (formatting, etc.)
- `refactor`: Code change that neither fixes a bug nor adds a feature
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Changes to build process or auxiliary tools

### Examples

```
feat(auth): add OAuth2 authentication

Implement OAuth2 authentication flow with support for
Google and GitHub providers.

Closes #123
```

```
fix(api): resolve race condition in user lookup

The user lookup was susceptible to race conditions when
multiple requests were made simultaneously.

Fixes #456
```

## Questions?

If you have questions about contributing, please open an issue with the "question" label.

Thank you for contributing! 🎉
