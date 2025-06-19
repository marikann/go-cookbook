---
title: 'Goroutines in Go'
description: 'Learn how to leverage goroutines for concurrent execution in Go'
date: '2025-03-24'
category: 'Concurrency'
---

Go provides powerful support for concurrency through goroutines, lightweight threads managed by the Go runtime. This snippet demonstrates how to use goroutines to execute concurrent tasks effectively.

## Basic Goroutine Usage

Here's a simple example that shows how to launch a goroutine:

```go
package main

import (
	"fmt"
	"time"
)

func processData() {
	for i := 0; i < 5; i++ {
		fmt.Printf("Processing data: %d\n", i)
		time.Sleep(1 * time.Second)
	}
}

func main() {
	go processData() // Launches processData in a new goroutine.

	time.Sleep(6 * time.Second) // Allow time for the goroutine to finish.
	fmt.Println("Processing complete")
}
```

## Using Goroutines with Anonymous Functions

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go func() {
		for i := 'a'; i <= 'e'; i++ {
			fmt.Printf("%c", i)
			time.Sleep(500 * time.Millisecond)
		}
	}()

	time.Sleep(3 * time.Second) // Allow time for the goroutine to finish.
	fmt.Println("\nProcessing complete")
}
```

## Parallelizing Tasks with Goroutines

```go
package main

import (
	"fmt"
	"time"
)

func worker(id int, work chan int, done chan bool) {
	for n := range work {
		fmt.Printf("Worker %d processing task %d\n", id, n)
		time.Sleep(time.Second)
		done <- true
	}
}

func main() {
	work := make(chan int, 5)
	done := make(chan bool, 5)

	for i := 1; i < 5; i++ {
		go worker(i, work, done)
	}

	for j := 0; j < 5; j++ {
		work <- j
	}
	close(work)

	for k := 0; k < 5; k++ {
		<-done
	}
	fmt.Println("All tasks processed")
}
```

## Best Practices

- **Synchronization**: Use synchronization primitives like channels, wait groups, or mutexes to coordinate goroutines.
- **Channel Avoidance**: Avoid using channels for excessive internal communication; prefer shared memory with proper synchronization.
- **Cancellation**: Implement cancellation for goroutines using contexts to avoid goroutines running indefinitely.
- **Resource Limitation**: Limit the number of active goroutines to control resource consumption effectively.

## Common Pitfalls

- **Resource Leaks**: Goroutines can cause memory leaks if they block indefinitely. Ensure proper cleanup by closing channels or using timeouts.
- **Race Conditions**: Without synchronization, shared data may lead to race conditions. Use the `sync` package to protect shared resources.
- **Deadlocks**: Pay attention to blocking operations like channel sends/receives which can lead to deadlocks if not managed appropriately.
- **Panic Handling**: Uncaught panics in goroutines may terminate the program. Use `recover` to handle them safely in production.

## Performance Tips

- **Batch Processing**: When processing large data sets, consider batching tasks to reduce synchronization overhead and allow concurrent processing.
- **Load Balancing**: Distribute work evenly across goroutines to maximize utilization and performance.
- **Lazy Initialization**: Delay the creation of goroutines until necessary to conserve resources.
- **Profiling**: Use Go's built-in profiler to identify bottlenecks and optimize concurrent code.
- **Reduce Context Switching**: Keep goroutines simple and focus on minimizing blocking operations to reduce context switching overhead. 

By following these tips and examples, you can effectively utilize goroutines in Go to enhance your application's performance and responsiveness.