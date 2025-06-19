---
title: 'Table Tests'
description: 'Learn how to implement table-driven tests in Go for comprehensive and maintainable unit testing.'
date: '2025-03-24'
category: 'Testing'
---

Table-driven tests are a powerful pattern in Go that facilitate writing comprehensive and easy-to-maintain tests. By using a slice of test cases, table-driven tests allow you to define multiple scenarios in a concise manner.

## Basic Table Test

Here is an example of a basic table-driven test in Go:

```go
package math

import (
	"testing"
)

// add function simply adds two integers.
func add(a, b int) int {
	return a + b
}

func TestAdd(t *testing.T) {
	tests := []struct {
		name     string
		a        int
		b        int
		expected int
	}{
		{"Add positive numbers", 2, 3, 5},
		{"Add with zero", 2, 0, 2},
		{"Add negative numbers", -2, -3, -5},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			result := add(tt.a, tt.b)
			if result != tt.expected {
				t.Errorf("add(%d, %d) = %d; want %d", tt.a, tt.b, result, tt.expected)
			}
		})
	}
}
```

## Structuring Tests with Subtests

This approach can be enhanced using subtests to give more granular control and readability:

```go
package math

import (
	"testing"
)

// multiply function multiplies two integers.
func multiply(a, b int) int {
	return a * b
}

func TestMultiply(t *testing.T) {
	tests := []struct {
		name     string
		a        int
		b        int
		expected int
	}{
		{"Multiply positive numbers", 2, 3, 6},
		{"Multiply by zero", 5, 0, 0},
		{"Multiply negative numbers", -2, -3, 6},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			result := multiply(tt.a, tt.b)
			if result != tt.expected {
				t.Errorf("multiply(%d, %d) = %d; want %d", tt.a, tt.b, result, tt.expected)
			}
		})
	}
}
```

## Advanced Table Testing with Test Fixtures

Using setup and teardown logic with table-driven tests can be implemented by embedding that logic directly:

```go
package math

import (
	"testing"
)

func TestComplexCalculation(t *testing.T) {
	tests := []struct {
		name     string
		setup    func() int
		cleanup  func()
		expected int
	}{
		{
			name: "Setup returns initial value",
			setup: func() int {
				// Setup logic here.
				return 42
			},
			cleanup: func() {
				// Cleanup logic here.
			},
			expected: 42,
		},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			// Setup.
			initialValue := tt.setup()

			// Perform calculation (Example: add a constant).
			result := initialValue + 0

			// Cleanup.
			tt.cleanup()

			if result != tt.expected {
				t.Errorf("Result = %d; want %d", result, tt.expected)
			}
		})
	}
}
```

## Best Practices

- Use descriptive test names to understand the test's purpose at a glance.
- Leverage Go's subtests for better granularity and reporting, especially when using `go test -v`.
- Separate setup and cleanup logic to keep your test structure clean and maintainable.
- Encapsulate repetitive logic in helper functions to reduce boilerplate in test cases.

## Common Pitfalls

- Avoid overly complex test cases within your table, which can make it harder to identify the purpose of each test.
- Ensure that test data does not interfere between different test cases, especially when they rely on shared resources.
- Neglecting the use of subtests can make it difficult to diagnose which specific case failed during execution.

## Performance Tips

- For performance-sensitive tests, minimize setup/teardown complexity to reduce the test execution time.
- Keep test cases atomic and focused to enable effective parallel execution.
- Use go test benchmarking features (`b.N`) when you need to profile functions within the context of a test.