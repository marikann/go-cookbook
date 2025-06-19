---
title: 'Project Layout Conventions in Go'
description: 'Discover the standard project layout conventions in Go and learn how to organize your Go projects effectively.'
date: '2025-03-24'
category: 'Tools'
---

The Go community follows certain conventions when it comes to project layout to ensure consistency, readability, and maintainability. This guide highlights these conventions and illustrates how to structure your Go projects effectively.

## Basic Project Layout

A typical Go project is structured to differentiate between packages and executable commands. Here’s a basic example:

```
your-project/
    ├── cmd/
    │   ├── yourapp/
    │   │   └── main.go
    ├── pkg/
    │   ├── package1/
    │   │   └── package1.go
    │   ├── package2/
    │       └── package2.go
    ├── internal/
    │   ├── helper/
    │       └── helper.go
    ├── go.mod
    └── README.md
```

### Explanation

- **cmd/**: This directory contains the main application entry point(s). Each subdirectory represents a different command or application. In this example, `yourapp` is the main application.

- **pkg/**: This directory is used for library code that you want to expose/allow other projects to import. It’s a public package, adhering to the Go philosophy that emphasizes reusable packages.

- **internal/**: This directory allows you to declare packages that are only usable by your code. Code here is inaccessible from outside this project, which is useful for non-public packages.

- **go.mod**: The `go.mod` file is essential for Go modules. It defines the module path and its dependencies.

- **README.md**: Documentation for your project, providing an overview and guidance for users.

## Example Go Project

Below is an example of a simple Go project set up with the Go module system:

```go
// file: cmd/yourapp/main.go
package main

import (
	"fmt"
	"your-project/internal/helper"
	"your-project/pkg/package1"
)

func main() {
	fmt.Println("Welcome to YourApp!")
	helper.DoSomething()
	package1.PerformAction()
}
```

Here's what the supporting code looks like in `internal` and `pkg` packages:

```go
// file: internal/helper/helper.go
package helper

import "fmt"

func DoSomething() {
	fmt.Println("Doing something internally.")
}
```

```go
// file: pkg/package1/package1.go
package package1

import "fmt"

func PerformAction() {
	fmt.Println("Performing action from package1.")
}
```

## Best Practices

- **Modular Code Structure**: Place commonly used packages in `/pkg` for reusability. Use `/internal` to protect code from being reused externally if necessary.
- **Package Naming**: Use short, meaningful package names that reflect their functionality. Avoid using underscores or camel case in package names.
- **Clear Separation**: Separate executable applications and library packages to make the project structure self-explanatory.

## Common Pitfalls

- **Flat Structure**: Avoid flat directory structures which can lead to confusion and a lack of modularity.
- **Over-Engineering**: Don't excessively split your code into packages if they are not necessary; it can lead to cognitive overload and increased maintenance.
- **Ignoring Conventions**: Not following Go's project layout conventions can lead to confusion for contributors and others reading your code.

## Performance Tips

- **Optimize Imports**: Use only the necessary packages to keep build times low.
- **Compiler Directives**: Use build constraints (tags) to conditionally compile code, improving performance by excluding unnecessary code.
- **Concurrency**: Limit the number of dependencies in high-performance applications to enhance build times and reduce binary sizes.

By adhering to these layout conventions, you ensure that your Go projects are clean, maintainable, and easily understandable by other developers.