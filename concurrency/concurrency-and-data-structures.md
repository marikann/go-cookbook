---
title: 'Concurrency and Data Structures'
description: 'Explore concurrency strategies with data structures in Go to achieve safe and efficient access and modifications.'
date: '2025-03-24'
category: 'Concurrency'
---

In concurrent programming, ensuring safe access and modification of data structures is crucial to avoid race conditions and ensure data integrity. Go offers several synchronization primitives and features to handle concurrency effectively.

## Using Mutex with Maps

Maps in Go are not safe for concurrent use by default. To safely access a map concurrently, you can use `sync.Mutex`.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.Mutex
	m := make(map[string]int)

	wg := sync.WaitGroup{}
	for i := 0; i < 5; i++ {
		wg.Add(1)
		go func(i int) {
			defer wg.Done()
			mu.Lock()
			m[fmt.Sprintf("task%d", i)] = i
			mu.Unlock()
		}(i)
	}
	wg.Wait()

	mu.Lock()
	fmt.Println("Data contents:", m)
	mu.Unlock()
}
```

## Using RWMutex for Read/Write Optimization

When there are frequent reads and fewer writes, using `sync.RWMutex` can improve performance by allowing multiple goroutines to read simultaneously while still maintaining write safety.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var rwMu sync.RWMutex
	m := make(map[string]int)

	wg := sync.WaitGroup{}
	for i := 0; i < 5; i++ {
		wg.Add(1)
		go func(i int) {
			defer wg.Done()
			rwMu.Lock()
			m[fmt.Sprintf("task%d", i)] = i
			rwMu.Unlock()
		}(i)
	}

	wg.Add(1)
	go func() {
		defer wg.Done()
		rwMu.RLock()
		fmt.Println("Reading data contents:", m)
		rwMu.RUnlock()
	}()

	wg.Wait()
}
```

## Using Sync.Map

`sync.Map` is a concurrent map that does not require manual locking. It is optimized for scenarios with many concurrent goroutines accessing the same map.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var sm sync.Map

	wg := sync.WaitGroup{}
	for i := 0; i < 5; i++ {
		wg.Add(1)
		go func(i int) {
			defer wg.Done()
			sm.Store(fmt.Sprintf("task%d", i), i)
		}(i)
	}

	wg.Wait()

	sm.Range(func(key, value interface{}) bool {
		fmt.Printf("%s: %d\n", key, value)
		return true
	})
}
```

## Best Practices

- Use `sync.Mutex` for critical sections where both read and write access must be safe.
- Prefer `sync.RWMutex` for data structures where reads significantly outnumber writes, to allow parallel reads.
- Consider using `sync.Map` for highly concurrent scenarios, but be aware that its performance might not always be better than a `map` with explicit locking.

## Common Pitfalls

- Forgetting to unlock a `Mutex` or `RWMutex` when a function returns early can lead to deadlocks.
- Using `sync.Map` for all scenarios indiscriminately, as it may not always provide the best performance due to increased operational overhead.
- Incorrectly assuming `map` is safe for concurrent read/writes without synchronization, which leads to subtle race conditions.

## Performance Tips

- Use fine-grained locking instead of a single global lock to reduce contention in highly parallelized applications.
- Profile your application to determine the exact performance characteristics; sometimes synchronization overhead might outweigh concurrent map advantages.
- Take advantage of `sync.Pool` for reducing allocation overhead in concurrent data structure usage.