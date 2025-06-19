---
title: 'Understanding Context Chains'
description: 'Learn how to effectively use context chains in Go to manage cancellations and timeouts across goroutines'
date: '2025-03-24'
category: 'Context'
---

Go's `context` package is crucial for handling cancellations, deadlines, and values across API boundaries and controlling goroutines. In this snippet, we will explore context chains, which allow you to propagate cancellation and timeouts effectively.

## Basic Context Chain Usage

Creating a root context and deriving child contexts:

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func main() {
	// Create a root context.
	ctx := context.Background()

	// Create a derived context with a timeout of 2 seconds.
	ctx, cancel := context.WithTimeout(ctx, 2*time.Second)
	defer cancel()

	ch := make(chan int)

	// Simulate a long-running operation.
	go func(ctx context.Context) {
		select {
		case <-time.After(1 * time.Second): // Simulate some workload
			ch <- 73
		case <-ctx.Done():
			return // Exit if context is cancelled
		}
	}(ctx)

	select {
	case result := <-ch:
		fmt.Println("Processing result:", result)
	case <-ctx.Done():
		fmt.Println("Operation cancelled:", ctx.Err())
	}
}
```

## Context Chains with Deadlines

Handling timeouts and cancellations cleanly using context chains:

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func main() {
	ctx := context.Background()
	deadline := time.Now().Add(1 * time.Second)
	ctx, cancel := context.WithDeadline(ctx, deadline)
	defer cancel()

	go processRequest(ctx)

	time.Sleep(2 * time.Second) // Simulate delay
}

func processRequest(ctx context.Context) {
	select {
	case <-ctx.Done():
		fmt.Println("Request cancelled:", ctx.Err())
		return
	case <-time.After(500 * time.Millisecond): // Simulate processing time
		fmt.Println("Data processing completed")
	}
}
```

## Best Practices

- **Defer Cancel**: Always `defer cancel()` immediately after creating a context with a cancellation function to ensure that resources are released promptly.
- **Context Cancellation**: Use context to propagate cancellations across goroutines efficiently, improving resource management.
- **Limit Context Values**: Avoid storing large amounts of data in context values since they are only intended for request-scoped data, not sharing data.

## Common Pitfalls

- **Misuse of Background and TODO**: Use `context.Background()` for top-level contexts and `context.TODO()` as a placeholder during refactoring; don't use them interchangeably.
- **Ignoring Context Errors**: Always check `ctx.Err()` to determine why a context was canceled or exceeded its deadline.
- **Avoiding Time.Sleep**: Use context timeouts instead of `time.Sleep()` for goroutine management to handle cancellation and timeouts cleanly.

## Performance Tips

- **Efficient Chain Management**: Rather than creating multiple independent contexts, link contexts in a chain to avoid excessive resource use.
- **Profile Context Lifetimes**: Periodically audit your context usage to ensure there are no memory leaks due to contexts not being canceled or properly garbage-collected.
- **Use Context for Request Scopes Only**: Limit the use of context to controlling request lifetimes without clogging the system with unnecessary operations.