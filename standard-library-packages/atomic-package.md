---
title: 'Atomic Package'
description: 'Learn how to perform atomic operations in Go using the sync/atomic package'
date: '2025-03-24'
category: 'Standard Library Packages'
---

The `sync/atomic` package in Go provides low-level atomic memory primitives which are crucial for building concurrent applications without race conditions. These operations are essential when you need to change a shared variable between multiple goroutines safely.

## Atomic Integer Operations

Here’s an example showcasing how to safely increment and read an integer value using atomic operations:

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {
	var counter int32
	var wg sync.WaitGroup

	wg.Add(2) // Two goroutines incrementing the counter

	// Goroutine to increment the counter.
	go func() {
		defer wg.Done()
		for i := 0; i < 1000; i++ {
			atomic.AddInt32(&counter, 1)
		}
	}()

	// Another goroutine to increment the counter.
	go func() {
		defer wg.Done()
		for i := 0; i < 1000; i++ {
			atomic.AddInt32(&counter, 1)
		}
	}()

	wg.Wait()
	fmt.Println("Final Counter:", atomic.LoadInt32(&counter)) // Safely read the counter
}
```

## Atomic Boolean Operations

Here is how you can work with boolean variables atomically:

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
)

func main() {
	var flag int32
	var wg sync.WaitGroup

	wg.Add(1)

	go func() {
		defer wg.Done()
		// Atomically set the flag to true.
		atomic.StoreInt32(&flag, 1)
	}()

	wg.Wait()

	if atomic.LoadInt32(&flag) == 1 {
		fmt.Println("Flag is set to true")
	} else {
		fmt.Println("Flag is false")
	}
}
```

## Best Practices

- Use atomic operations for simple shared memory updates and reads to avoid using locks.
- Prefer high-level concurrency controls (like `sync.Mutex`) if operations become complex or involve multiple variables.
- Always use atomic operations when modifying variables accessed by multiple goroutines.

## Common Pitfalls

- Forgetting to use atomic operations on variables accessed by multiple goroutines can lead to race conditions.
- Atomic operations should only be used when working with basic types like integers and pointers. Complex operations might require locking mechanisms for consistency.
- Avoid mixing atomic operations and non-atomic reads/writes on the same variable.

## Performance Tips

- Direct atomic loads and stores are generally faster than lock-based mechanisms for simple operations involving single variables.
- Optimize atomic operations to minimize the retries in case of contention, as this can degrade performance.
- Use atomic operations when performance is a concern and locking might introduce unnecessary overhead.