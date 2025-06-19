---
title: 'Parallel Test Execution'
description: 'Learn how to execute tests in parallel in Go to reduce testing time and improve efficiency.'
date: '2025-03-24'
category: 'Testing'
---

Parallel test execution allows you to run multiple tests simultaneously, reducing the total time required to execute a test suite. In Go, you can achieve parallel test execution using the built-in testing package.

## Basic Parallel Test Execution

Here's a simple example demonstrating how to execute tests in parallel using Go's `testing` package:

```go
package main

import (
	"testing"
	"time"
)

// Example test function that runs in parallel.
func TestExampleParallel(t *testing.T) {
	t.Parallel() // This indicates that this test is safe to run in parallel

	time.Sleep(1 * time.Second) // Simulate a test that takes 1 second
	t.Log("TestExampleParallel finished")
}

// Another example test function.
func TestAnotherExampleParallel(t *testing.T) {
	t.Parallel()

	time.Sleep(2 * time.Second) // Simulate a longer test
	t.Log("TestAnotherExampleParallel finished")
}
```

## Running Sub-tests in Parallel

Go also allows sub-tests to be run in parallel. Here's how you can write tests with sub-tests that execute concurrently:

```go
package main

import (
	"testing"
	"time"
)

func TestSubtestsParallel(t *testing.T) {
	tests := []struct {
		name string
	}{
		{"test1"},
		{"test2"},
		{"test3"},
	}

	for _, tt := range tests {
		tt := tt // capture range variable
		t.Run(tt.name, func(t *testing.T) {
			t.Parallel()
			time.Sleep(1 * time.Second)
			t.Logf("%s finished", tt.name)
		})
	}
}
```

## Best Practices

- **Avoid Shared State:** Ensure that tests do not modify shared state, or use synchronization primitives if shared state is necessary.
- **Use `t.Parallel()` Judiciously:** Only mark tests as parallel if they are truly independent and do not affect each other, such as tests that do not modify global state.
- **Resource Management:** Consider the limits of your system resources (CPU, memory) when running tests in parallel to avoid resource exhaustion.

## Common Pitfalls

- **Race Conditions:** Running tests in parallel can introduce race conditions if tests access shared data. Use Go's race detector (with `go test -race`) to identify and fix such issues.
- **Test Order Dependence:** Ensure that tests do not depend on the execution order, as parallel execution can change the order in which tests are run.
- **Resource Contention:** Be vigilant about tests competing for the same resources, which can lead to timeout failures.

## Performance Tips

- **Limited Parallelism:** You can control the degree of parallelism using the `-parallel` flag with `go test`. Set it according to your hardware's capability to optimize performance (`go test -parallel=4`).
- **Profile Test Runs:** Use profiling tools to understand and optimize the performance of your test suite when parallelizing.
- **Balance Test Duration:** Aim to balance the duration of parallel tests so that no single test becomes a bottleneck, ensuring optimal resource usage and reduced execution time.