---
title: 'Using gosec Tool'
description: 'Learn how to secure your Go code by using the gosec tool to find common vulnerabilities.'
date: '2025-03-24'
category: 'Security'
---

The `gosec` tool is a static code analyzer that helps identify security issues in Go projects. It scans the source code and reports potential vulnerabilities, ensuring your Go applications remain secure.

## Installing gosec

To start using gosec, first install it using the following command:

```bash
go install github.com/securego/gosec/v2/cmd/gosec@latest
```

Ensure that your Go workspace's `bin` directory is part of your `PATH` to run the `gosec` command from anywhere.

## Basic Usage

To scan your project directory with gosec and produce a security report, execute the following command from the root of your project:

```bash
gosec ./...
```

This will analyze all packages in your project and report any security issues found.

## Output Analysis

Here's an example of what the output might look like:

```
[gosec] 2023/03/24 16:00:00 Including rules: default
[gosec] 2023/03/24 16:00:00 Excluding rules: default
[gosec] 2023/03/24 16:00:00 Import directory: /path/to/project
Results:

[/path/to/project/main.go:10] - Possible hardcoded credentials (gosec) (CWE-798):
   9:    username := "admin"
  10:    password := "password123"

[/path/to/project/util.go:22] - GoSec Exception (CWE-703):
  21:   log.Fatal(err)

Summary:
   Files: 10
   Lines: 237
   Issues: 2
```

Each issue found will provide the file, line number, a description, and a CWE (Common Weakness Enumeration) identifier.

## Customizing the Scan

You can customize the scan by including or excluding certain checks using the `-include` or `-exclude` flags:

```bash
gosec -include=G101,G201 ./...
```

To focus on specific vulnerability types or exclude false positives.

## Generating Reports

Generate reports in different formats, such as JSON, CSV, or JUnit XML, using the `-fmt` flag:

```bash
gosec -fmt=json -out=report.json ./...
```

These reports can be integrated into CI/CD pipelines for automatic security checks.

## Best Practices

- Regularly update `gosec` to use the latest rules and detections.
- Integrate `gosec` into your CI pipeline to ensure every commit is checked for vulnerabilities.
- Review and address each finding, particularly those with high severity or affecting sensitive parts of your application.

## Common Pitfalls

- Ignoring gosec findings because of false positives; instead, use the `-exclude` flag or suppress specific lines with inline comments.
- Relying solely on gosec for security measures; combine it with other tools and practices such as code reviews and penetration testing.
- Not customizing gosec scans for your specific application needs, which can lead to missing critical issues.

## Performance Tips

- Use the `-concurrency` flag to specify the number of concurrent goroutines used by gosec, optimizing scan speed on larger projects.
- For significantly large projects, exclude third-party directories that are outside the scope of your application, focusing scan efforts on your code.
- Preemptively profile large codebases with `gosec` to understand and mitigate overhead before integrating into continuous integration workflows.