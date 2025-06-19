---
title: 'errors.As and errors.Is'
description: 'Understand how to work with errors in Go using errors.As and errors.Is for type and value comparison.'
date: '2025-03-24'
category: 'Tools'
---

In Go, error handling is a fundamental aspect of robust code. Starting from Go 1.13, the `errors` package introduced two powerful functions, `errors.As` and `errors.Is`, which help in handling and analyzing errors more effectively.

## Using errors.Is

The `errors.Is` function checks if an error is, or wraps, a specific target error. This is useful for comparing errors that are wrapped within other errors using `fmt.Errorf` with `%w`.

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	originalErr := errors.New("network failure")
	wrappedErr := fmt.Errorf("operation failed: %w", originalErr)

	if errors.Is(wrappedErr, originalErr) {
		fmt.Println("The error is a network failure.")
	} else {
		fmt.Println("The error is of a different type.")
	}
}
```

## Using errors.As

The `errors.As` function is used to test whether an error is or wraps a specific error type. It can be used especially when dealing with custom error types.

```go
package main

import (
	"errors"
	"fmt"
)

type CustomError struct {
	Code int
	Msg  string
}

func (c CustomError) Error() string {
	return fmt.Sprintf("Error %d: %s", c.Code, c.Msg)
}

func main() {
	customErr := CustomError{Code: 404, Msg: "Not Found"}
	wrappedErr := fmt.Errorf("operation failed: %w", customErr)

	var err CustomError
	if errors.As(wrappedErr, &err) {
		fmt.Printf("Custom error found: %d, %s\n", err.Code, err.Msg)
	} else {
		fmt.Println("Error is not of type CustomError.")
	}
}
```

## Best Practices

- Use `errors.Is` for comparing static error values and ensure that errors are wrapped with `%w`.
- Use `errors.As` for error types when you need to extract additional context or information specific to those custom error types.

## Common Pitfalls

- Forgetting to use `%w` to wrap errors in `fmt.Errorf`, which will lead to `errors.Is` and `errors.As` not functioning as expected.
- Not checking for error types correctly, which could lead to misleading error handling.

## Performance Tips

- Only perform deep error analysis (like type assertion with `errors.As`) when necessary to avoid performance hits in high-throughput or low-latency applications.
- Ensure that the error types you define or expect are used judiciously to avoid excessive complexity in error handling.