---
title: 'Atomic Types in Go'
description: 'Learn how to use atomic types in Go for safe concurrent programming with the sync/atomic package.'
date: '2025-03-24'
category: 'Tools'
---

Go's `sync/atomic` package provides low-level atomic memory primitives useful for building complex concurrent algorithms. These atomic types allow safe, lock-free operations on shared data, ideal for performance-critical applications.

## Basic Usage of Atomic Types

Here’s how you can use atomic types for safe concurrent increments:

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {
	var value int32
	var wg sync.WaitGroup

	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			atomic.AddInt32(&value, 1)
		}()
	}

	wg.Wait()
	fmt.Printf("Final value: %d\n", value)
}
```

## Using Atomic Types for Booleans

Go doesn't have an atomic boolean, but you can emulate it with an `int32`:

```go
package main

import (
	"fmt"
	"sync/atomic"
)

func main() {
	var flag int32 = 0

	// Set the flag to true.
	atomic.StoreInt32(&flag, 1)

	// Check if the flag is true.
	if atomic.LoadInt32(&flag) == 1 {
		fmt.Println("Processing flag is set")
	}
}
```

## Compare and Swap (CAS) Operation

CAS is a powerful operation for implementing lock-free data structures safely:

```go
package main

import (
	"fmt"
	"sync/atomic"
)

func main() {
	var value int32 = 42

	swapped := atomic.CompareAndSwapInt32(&value, 42, 73)
	fmt.Printf("Swapped: %v, Value: %d\n", swapped, value)
}
```

## Best Practices

- Use atomic operations when you need lock-free, high-performance concurrency control.
- Ensure atomic operations are appropriate for your data size and type; use locks if atomic types don't fit.
- Opt for `atomic.Value` for complex types, providing safe and atomic load/store operations.

## Common Pitfalls

- Avoid using atomic operations with types over their intended size (e.g., more than 64 bits) as it can lead to undefined behavior.
- Relying solely on atomic operations for complex logic may lead to subtle bugs and should be limited to performance-critical sections.
- Ensure that all shared data manipulations are atomic or appropriately synchronized.

## Performance Tips

- Prefer atomic operations over mutexes for simple counters as they provide lock-free performance benefits.
- Consider the overhead of CAS operations in performance-sensitive paths; if high contention is expected, performance may degrade.
- Use profiling tools to identify bottlenecks and decide between atomic operations and locks based on contention levels.