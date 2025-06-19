---
title: 'Versioning Go Modules'
description: 'Learn how to properly version Go modules using semantic versioning and the Go command line tools.'
date: '2025-03-24'
category: 'Modules'
---

Go modules are versioned using semantic versioning (SemVer), which is crucial for resolving dependencies and ensuring compatibility. Proper versioning allows your modules to be easily consumed by others while maintaining stability.

## Basic Versioning with Go Modules

Here’s how you can create a versioned module with Go:

### Initialize a New Module

Start by creating a new Go module and initialize it with `go mod init`:

```bash
mkdir mymodule
cd mymodule
go mod init github.com/yourusername/mymodule
```

### Create and Tag a New Version

Create or modify code in your module and commit the changes. Once ready for a release, tag it with a semantic version:

```bash
git add .
git commit -m "Initial release"
git tag v1.0.0
```

Push your commit and tags to a remote repository:

```bash
git push origin main --tags
```

This ensures others can easily access your module.

## Updating Module Versions

When making changes that need versioning, follow these SemVer rules:

- **Major**: Increment when you make incompatible API changes.
- **Minor**: Increment when you add functionality in a backward-compatible manner.
- **Patch**: Increment for backward-compatible bug fixes.

### Example: Releasing a New Version

After making backward-compatible changes:

```bash
# Make changes, then...
git add .
git commit -m "Add a new feature"
git tag v1.1.0
git push origin main --tags
```

## Advanced Versioning Commands

Use the `go` command to manage and inspect versions:

- **Listing Module Versions**: To list all available versions of a module, use:

  ```bash
  go list -m -versions github.com/yourusername/mymodule
  ```

- **Upgrading Dependencies**: To upgrade dependencies in your project:

  ```bash
  go get github.com/otherusername/othermodule@latest
  ```

- **Releasing Pre-release or Build Metadata Versions**: If you want to use pre-release versions:

  ```bash
  git tag v1.1.0-beta+exp.sha.5114f85
  ```

## Best Practices

- **Use Semantic Versioning Seriously**: Stick to SemVer rules strictly to ensure your users experience minimal compatibility issues.
- **Automate Releases**: Use CI/CD tools to automate the tagging and publishing of releases.
- **Document Changes Clearly**: Maintain a clear changelog for each release to inform users of changes.

## Common Pitfalls

- **Skipping Version Numbers**: Avoid skipping minor or patch numbers; it can confuse users and systems managing dependencies.
- **Premature Major Versions**: Releasing a major version too early can complicate future compatibility.
- **Inconsistent Tags**: Ensure your git tags are consistent and have matching Go module versions.

## Performance Tips

- **CI/CD for Tagged Releases**: Automatically test your module upon tagging a release for immediate feedback.
- **Minimal Module Dependencies**: Keep dependencies up-to-date and minimal to reduce complexities and potential performance issues.
- **Immutable Tags**: Once a tag is created, do not force push changes to that tag; instead, create a new version to address issues.

Implement these practices to perfectly manage and version your Go modules, providing reliability and ease of use for contributors and users alike.