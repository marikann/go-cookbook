---
title: 'Creating Custom Error Types'
description: 'Learn how to define and use custom error types in Go to improve error handling and clarity.'
date: '2025-03-24'
category: 'Tools'
---

In Go, error handling is a critical aspect of programming. While Go's built-in `error` interface is sufficient for many cases, creating custom error types can provide more context and clarity, especially in complex applications.

## Defining a Basic Custom Error Type

Creating a custom error type involves defining a new type and implementing the `Error()` method from the `error` interface.

```go
package main

import (
	"fmt"
)

type MyError struct {
	Code    int
	Message string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("Code %d: %s", e.Code, e.Message)
}

func faultyOperation() error {
	return &MyError{Code: 404, Message: "Resource not found"}
}

func main() {
	err := faultyOperation()
	if err != nil {
		fmt.Println(err)
	}
}
```

## Enhancing Custom Errors with Additional Context

You can add more contextual information to your custom error type to aid in debugging and error handling.

```go
package main

import (
	"fmt"
)

type ValidationError struct {
	Field   string
	Message string
}

func (e *ValidationError) Error() string {
	return fmt.Sprintf("Validation Error - Field: %s, Message: %s", e.Field, e.Message)
}

func validateField(field string, value string) error {
	if value == "" {
		return &ValidationError{Field: field, Message: "cannot be empty"}
	}
	return nil
}

func main() {
	err := validateField("Username", "")
	if err != nil {
		fmt.Println(err)
	}
}
```

## Wrapping Errors with Additional Context

Using the `fmt.Errorf` function and the `%w` verb, you can wrap existing errors with additional information.

```go
package main

import (
	"errors"
	"fmt"
)

func operation() error {
	return errors.New("underlying error")
}

func wrappedOperation() error {
	err := operation()
	if err != nil {
		return fmt.Errorf("wrapped operation error: %w", err)
	}
	return nil
}

func main() {
	err := wrappedOperation()
	if err != nil {
		fmt.Println(err)
	}
}
```

## Best Practices

- Implement the `Error()` method on your custom error types to return meaningful messages.
- Use custom error types to capture additional context such as error codes, field names, or additional messages.
- Consider using error wrapping to maintain a chain of errors through function calls, providing a complete picture of the error context.

## Common Pitfalls

- Forgetting to implement the `Error()` method, which can lead to unhelpful error messages.
- Ignoring the original error when wrapping, which might lose valuable information.
- Creating overly specific error types that cause unnecessary complexity in error handling logic.

## Performance Tips

- Use pointers for custom error types to avoid copying data structures unnecessarily.
- When wrapping errors, ensure error chains are not excessively long, which can make debugging difficult.
- Prefer using `errors.Is` and `errors.As` functions from the `errors` package to check wrapped errors instead of type assertions.