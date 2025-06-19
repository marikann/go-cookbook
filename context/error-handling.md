---
title: 'Error Handling in Go'
description: 'Understand error handling in Go with best practices, and examples using standard practices like the errors and fmt packages.'
date: '2025-03-24'
category: 'Context'
---

Error handling in Go is a critical part of writing robust applications. Go encourages errors to be treated as a value, supporting explicit error checks rather than relying on exception-based handling.

## Basic Error Handling

Go functions that might encounter an error conventionally return an additional `error` value to indicate failure:

```go
package main

import (
	"errors"
	"fmt"
)

func divide(a, b float64) (float64, error) {
	if b == 0 {
		return 0, errors.New("division by zero")
	}
	return a / b, nil
}

func main() {
	result, err := divide(73, 0)
	if err != nil {
		fmt.Println("Error:", err)
		return
	}
	fmt.Println("Result:", result)
}
```

## Using fmt.Errorf for Formatted Errors

The `fmt.Errorf` function is helpful for creating formatted error messages:

```go
package main

import (
	"fmt"
)

func findItem(items []string, target string) (string, error) {
	for _, item := range items {
		if item == target {
			return item, nil
		}
	}
	return "", fmt.Errorf("item %s not found", target)
}

func main() {
	item, err := findItem([]string{"golang", "docker", "kubernetes"}, "jenkins")
	if err != nil {
		fmt.Println("Error:", err)
		return
	}
	fmt.Println("Found item:", item)
}
```

## Wrapping Errors with the New Errors Package

Go 1.13 introduced enhanced error handling, allowing us to wrap errors:

```go
package main

import (
	"errors"
	"fmt"
)

func openFile(filename string) error {
	// Simulate an error in opening a file.
	return errors.New("failed to open file")
}

func main() {
	err := openFile("config.yaml")
	if err != nil {
		wrappedErr := fmt.Errorf("could not complete operation: %w", err)
		fmt.Println(wrappedErr)
	}
}
```

## Best Practices

- **Return errors as soon as possible:** Avoid delaying error checks; return early if an error occurs, handling them appropriately.
- **Use `fmt.Errorf` for detailed context:** When more context is needed, `fmt.Errorf` can provide insightful error messages.
- **Wrap errors to preserve the stack trace:** Wrapping errors helps in understanding the root cause when errors propagate through different layers.
- **Leverage Go's error handling capabilities from version 1.13:** Take advantage of wrapping and unwrapping features (`%w` placeholder) to maintain richer error chains.

## Common Pitfalls

- **Ignoring returned errors:** Always handle errors returned from functions to avoid unexpected behavior.
- **Improper wrapping of existing errors:** Not using `%w` in `fmt.Errorf` when wrapping results in loss of unwrapping capabilities.
- **Too generic error messages:** Generic errors without context make debugging difficult. Always aim to provide as much relevant context as possible.

## Performance Tips

- **Avoid creating new errors unnecessarily:** Creating errors consumes resources. Reuse known errors as constants for efficiency when the message can be generic.
- **Minimize wrapped error chains when possible:** Although wrapping errors provides valuable context, excessive error wrapping can lead to verbose logs. Strive for balance, especially in performance-critical paths.