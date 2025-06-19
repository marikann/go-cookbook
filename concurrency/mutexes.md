---
title: 'Mutexes'
description: 'Learn how to use mutexes in Go for synchronizing shared resources'
date: '2025-03-24'
category: 'Concurrency'
---

In concurrent programming, synchronization is crucial to ensure data integrity when multiple goroutines access shared resources. Go provides support for synchronization primitives, and one of the most common is the Mutex. The `sync` package provides the `Mutex` type which can help coordinate access to resources between goroutines.

## Using Mutex for Synchronization

Here's a basic example of using a mutex to protect access to a shared resource:

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.Mutex
	value := 0

	increment := func(wg *sync.WaitGroup, mu *sync.Mutex) {
		defer wg.Done()
		for i := 0; i < 1000; i++ {
			mu.Lock()
			value++
			mu.Unlock()
		}
	}

	var wg sync.WaitGroup

	wg.Add(2)
	go increment(&wg, &mu)
	go increment(&wg, &mu)

	wg.Wait()
	fmt.Printf("Final value: %d\n", value)
}
```

## Read-Write Mutex

For situations where many goroutines need read access but write operations are rare, a `sync.RWMutex` can be used to allow multiple concurrent reads but only one write:

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.RWMutex
	value := 0

	read := func(wg *sync.WaitGroup, mu *sync.RWMutex) {
		defer wg.Done()
		mu.RLock()
		defer mu.RUnlock()
		fmt.Println("Reading value:", value)
	}

	write := func(wg *sync.WaitGroup, mu *sync.RWMutex) {
		defer wg.Done()
		mu.Lock()
		value++
		mu.Unlock()
	}

	var wg sync.WaitGroup

	wg.Add(3)
	go write(&wg, &mu)
	go read(&wg, &mu)
	go read(&wg, &mu)

	wg.Wait()
}
```

## Best Practices

- Use `defer mu.Unlock()` to ensure the mutex is unlocked even if a panic occurs.
- Use `RWMutex` for data structures that have high read-to-write ratios.
- Keep the critical section (the code between `Lock` and `Unlock`) as short as possible to reduce contention.

## Common Pitfalls

- Forgetting to unlock the mutex, which can cause deadlocks.
- Overuse of mutexes leading to contention, affecting performance.
- Copying a mutex after it being used, which can cause a program to panic.
- Locking a mutex for too long, starving other goroutines.
  
## Performance Tips

- Avoid locking a mutex around long-running operations and I/O.
- Use channels for communication between goroutines when possible instead of mutexes.
- Benchmark with real-world data to tune performance (e.g., choosing between `Mutex` and `RWMutex`).
- Profile the application to detect heavy lock contention and optimize accordingly.

Using mutexes effectively is key to writing safe concurrent programs in Go. Remember to analyze your program's concurrency requirements carefully to choose the correct synchronization primitives.