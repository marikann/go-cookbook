---
title: 'Using Subtests'
description: 'Learn how to effectively use subtests in Go for better test organization and clarity.'
date: '2025-03-24'
category: 'Testing'
---

In Go, subtests provide an excellent mechanism to run related sets of tests with varying inputs and configurations. These help in organizing tests, especially when the logic being tested has multiple scenarios or configurations.

## Basic Subtest Usage

Here's a simple example that illustrates how to implement subtests when testing a sum function:

```go
package main

import (
	"testing"
)

func Sum(a, b int) int {
	return a + b
}

func TestSum(t *testing.T) {
	testCases := []struct{
		name string
		a, b int
		expected int
	}{
		{"both positive", 1, 2, 3},
		{"one negative", -1, 1, 0},
		{"both negative", -2, -3, -5},
	}

	for _, tc := range testCases {
		t.Run(tc.name, func(t *testing.T) {
			result := Sum(tc.a, tc.b)
			if result != tc.expected {
				t.Fatalf("expected %d, got %d", tc.expected, result)
			}
		})
	}
}
```

## Using Subtests for Setup and Teardown

```go
package main

import (
	"fmt"
	"testing"
)

func ConnectDB() string {
	return "Connected to DB"
}

func CloseDB() string {
	return "Closed DB connection"
}

func TestDatabase(t *testing.T) {
	setup := ConnectDB()
	fmt.Println(setup)
	defer func() {
		teardown := CloseDB()
		fmt.Println(teardown)
	}()
	
	testCases := []struct {
		name string
		query string
		result string
	}{
		{"select query", "SELECT * FROM table", "result1"},
		{"insert query", "INSERT INTO table values('data')", "result2"},
	}
	
	for _, tc := range testCases {
		t.Run(tc.name, func(t *testing.T) {
			// Dummy assertion for demonstration.
			if tc.query == "" {
				t.Errorf("query cannot be empty")
			}
		})
	}
}
```

## Best Practices

- **Descriptive Subtest Names**: Use descriptive names for subtests, which will aid in identification when a test fails.
- **Isolated Tests**: Ensure that subtests do not share dependencies unless necessary. This ensures they are isolated and less brittle.
- **Parallel Subtests**: Use `t.Parallel()` inside a subtest for tests that are independent and can be run concurrently to speed up testing.

## Common Pitfalls

- **Running Entire `Test` in Parallel**: Do not wrap the main test function (`TestXYZ`) with `t.Parallel()`. It is meant for subtest parallelization.
- **Indirect State Dependencies**: Be cautious with shared state across subtests. If a subtest alters shared state, ensure tests do not rely on each other’s state.
- **Fixtures and Teardown**: While using setup and teardown routines, ensure they are appropriately set for each subtest or shared safely.

## Performance Tips

- **Selective Subtest Execution**: Use `-run` flag with test function and subtest names to execute specific tests for faster debugging iterations.
- **Minimal State**: Avoid unnecessary setup in each subtest. Discard and efficiently reset state between subtests if needed.
- **Utilize Subtest Parallelism**: Apply `t.Parallel()` to independent subtests to capitalize on concurrent execution, reducing total test time.