---
title: 'Using the sync Package in Go'
description: 'Learn how to use the sync package for concurrency control and synchronization in Go applications.'
date: '2025-03-24'
category: 'Standard Library Packages'
---

The `sync` package in Go provides basic synchronization primitives essential for concurrent programming. This snippet demonstrates how to use these primitives, such as `Mutex`, `WaitGroup`, and `Once`.

## Using `sync.Mutex`

A `Mutex` is used to give safe access to data when different goroutines are reading and writing.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.Mutex
	counter := 0

	// Increment function.
	increment := func(wg *sync.WaitGroup) {
		defer wg.Done()
		mu.Lock()
		counter++
		mu.Unlock()
	}

	wg := &sync.WaitGroup{}

	for i := 0; i < 10; i++ {
		wg.Add(1)
		go increment(wg)
	}

	wg.Wait()
	fmt.Println("Counter:", counter)
}
```

## Using `sync.WaitGroup`

The `WaitGroup` is used to wait for a collection of goroutines to finish executing. 

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func worker(id int, wg *sync.WaitGroup) {
	defer wg.Done()
	fmt.Printf("Worker %d starting\n", id)
	time.Sleep(time.Second)
	fmt.Printf("Worker %d done\n", id)
}

func main() {
	const numJobs = 5
	var wg sync.WaitGroup

	for i := 1; i <= numJobs; i++ {
		wg.Add(1)
		go worker(i, &wg)
	}

	wg.Wait()
	fmt.Println("All workers completed")
}
```

## Using `sync.Once`

`sync.Once` is a concurrency primitive that ensures that a function is only executed once.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var once sync.Once

	setup := func() {
		fmt.Println("Setting up...")
	}

	for i := 0; i < 3; i++ {
		once.Do(setup)
		fmt.Println("Function executed", i+1, "times")
	}
}
```

## Best Practices

- Always use sync primitives such as mutexes around shared data to prevent race conditions.
- Use `sync.WaitGroup` to wait for goroutines to complete their tasks when you know beforehand how many goroutines you'll be launching.
- Apply `sync.Once` to initialize things such as configuration, database connections, etc., that should only occur once.

## Common Pitfalls

- Forgetting to call `Done()` on `WaitGroup`, which can cause `Wait()` to hang indefinitely.
- Using `Mutex` locks ineffectively or too broadly, which can lead to reduced performance or even deadlocks.
- Forgetting to handle the possible panic when using `Defer` with `Mutex.Unlock()`.

## Performance Tips

- Use `sync.Pool` if you have frequently used temporary objects to reduce the impact of garbage collection.
- Select the granularity of locking to achieve the right balance between safety and performance instead of locking entire functions or structs.
- When possible, prefer using `sync/atomic` for simple counters or flags to avoid the overhead of a `Mutex`.