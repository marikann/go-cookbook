---
title: 'Wrapping Errors'
description: 'Learn how to effectively wrap errors in Go using error handling features'
date: '2025-03-24'
category: 'Error Handling'
---

Go's error handling typically involves returning errors from functions. Wrapping errors adds additional context, making debugging easier. This snippet demonstrates how to wrap errors using Go’s standard library features.

## Basic Error Wrapping

The `fmt` package provides basic capabilities to wrap errors with additional context:

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	err := someFunction()
	if err != nil {
		fmt.Println(err)
	}
}

func anotherFunction() error {
	return errors.New("an error occurred")
}

func someFunction() error {
	err := anotherFunction()
	if err != nil {
		return fmt.Errorf("someFunction failed: %w", err)
	}
	return nil
}
```

## Using the `errors` Package for Wrapping

Go 1.13 introduced an enhancement to the `errors` package for error wrapping:

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	err := doSomething()
	if err != nil {
		fmt.Println(err)
	}
}

func faultyFunction() error {
	return errors.New("a critical failure")
}

func doSomething() error {
	err := faultyFunction()
	if err != nil {
		return fmt.Errorf("doSomething encountered an error: %w", err)
	}
	return nil
}
```

## Unwrapping Errors

To extract the original error after wrapping, use `errors.Unwrap` or `errors.Is` and `errors.As`:

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	err := performTask()
	if err != nil {
		if errors.Is(err, ErrSpecific) {
			fmt.Println("Found specific error:", ErrSpecific)
		} else {
			fmt.Println(err)
		}
	}
}

var ErrSpecific = errors.New("specific error")

func taskFunction() error {
	return ErrSpecific
}

func performTask() error {
	err := taskFunction()
	if err != nil {
		return fmt.Errorf("performTask failed: %w", err)
	}
	return nil
}
```

## Best Practices

- **Use `%w` for Wrapping**: Wrap errors using `%w` in `fmt.Errorf` to allow unwrapping.
- **Add Context**: When wrapping, ensure additional context is meaningful to aid debugging.
- **Maintain Error Chains**: Keep original errors in the chain for complete traceability.

## Common Pitfalls

- **Unwrapped Errors**: Failing to use `%w` results in unwrappable errors, breaking error chains.
- **Lack of Context**: Adding insufficient context makes debugging difficult and error messages obscure.
- **Overuse**: Avoid wrapping errors if it does not add significant value—overwrapping can clutter logs.

## Performance Tips

- **Avoid Excessive Wrapping**: Multiple unnecessary wraps can add overhead without providing additional benefits.
- **Use Built-in Support**: Rely on Go’s built-in error package’s features for efficiency rather than implementing custom solutions.
- **Profile Critical Sections**: Ensure error handling doesn’t become a bottleneck in performance-critical applications by profiling.