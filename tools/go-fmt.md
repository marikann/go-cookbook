---
title: Using go fmt
description: Explore how to format Go code automatically using the go fmt tool
date: 2025-03-24
category: Tools
---

`go fmt` is a powerful tool included with the Go programming language that automatically formats your Go source code in a standard style. This tool helps ensure consistency across your projects and makes collaborative development easier.

## Basic Usage of go fmt

Running `go fmt` is straightforward and can be done from the command line to format your Go files automatically:

```bash
# Format a single Go file
go fmt path/to/file.go

# Format all Go files in the current directory
go fmt ./...

# Format all Go files recursively in a specified directory
go fmt path/to/directory/...
```

## Integrating go fmt into Your Workflow

Integrating `go fmt` into your development process can save time and reduce errors. You can add the tool to your build or commit pipeline to ensure all code adheres to Go's standard format.

### Format on Save in Text Editors

Many popular text editors and IDEs support formatting Go code on save using `go fmt`. Configure your editor to apply the formatting automatically whenever you save a file.

### Git Hooks for go fmt

You can use a pre-commit Git hook to run `go fmt` and ensure your code is formatted before it gets committed:

Create a file named `pre-commit` in your `.git/hooks/` directory:

```bash
#!/bin/sh
# Format all Go files
go fmt ./...
```

Make the script executable:

```bash
chmod +x .git/hooks/pre-commit
```

## Best Practices

- Always format code before committing to a shared repository to ensure consistency and readability.
- Use editor extensions or configurations that automatically format code on save to integrate `go fmt` seamlessly into your workflow.
- Implement automatic formatting in CI/CD pipelines to maintain code quality across teams.

## Common Pitfalls

- Not running `go fmt` before pushing changes can lead to inconsistencies in code formatting.
- Assuming code is auto-formatted by default—ensure you're actively enforcing it across your projects.
- Not updating your file after formatting it; always review changes made by `go fmt`.

## Performance Tips

- Use `go fmt` frequently during development to avoid formatting issues piling up towards the end.
- Include a performance monitoring tool if `go fmt` impacts your build times, especially in large projects.
- Consider using `gofumpt` for more configurable formatting options while retaining compatibility with `go fmt`.

By using `go fmt`, you can maintain a consistent code style that helps make Go projects clean and understandable.